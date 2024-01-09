using System.Collections.Generic;
using System.Xml.Serialization;
using UnityEngine;

namespace ECFramework
{
    public class Entity : IEC
    {
        private GameObject gameObject;
        [XmlIgnore]
        public GameObject GameObject { get { return gameObject; } set { gameObject = value; GameObjectToEC.Add(this); } }
        [XmlIgnore]
        public Transform Transform { get { return gameObject == null ? null : gameObject.transform; } }
        public string DefName { get; set; }
        public string PrefebPath { get; set; }
        public Vector3 LocalPosition { get; set; }
        public List<Comp> Comps { get; set; }
        public List<Entity> SubEntities { get; set; }

        public GameObject GetOrCreateGameObject()
        {
            if (GameObject == null)
            {
                GameObject = new GameObject();
                GameObject.SetActive(false);
            }
            return GameObject;
        }

        public virtual void FirstCreate()
        {
            if (Comps != null)
            {
                foreach (var item in Comps)
                {
                    item.FirstCreate();
                }
            }
        }
        public virtual void SetReferences()
        {
            if (Comps != null)
            {
                foreach (var item in Comps)
                {
                    item.SetReferences();
                }
            }
        }
        public virtual void OnSpawn()
        {
            if (Comps != null)
            {
                foreach (var item in Comps)
                {
                    item.OnSpawn();
                }
            }
            if (GameObject != null)
            {
                GameObject.SetActive(true);
            }
        }

        public virtual void OnUnSpawn()
        {
            if (Comps != null)
            {
                foreach (var item in Comps)
                {
                    item.OnUnSpawn();
                }
            }
            if (GameObject != null)
            {
                GameObject.SetActive(false);
            }
        }
    }
}
