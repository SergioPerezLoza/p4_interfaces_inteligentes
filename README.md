# p4_interfaces_inteligentes

## alu0101473260@ull.edu.es

### Sergio Pérez Lozano

**Ejercicio 1**:
Cuando el Cubo colisione con el Cilindro, las esferas de Tipo 1 comenzarán a moverse hacia las esferas de Tipo 2, mientras que las esferas de Tipo 2 se moverán hacia el Cilindro.
![video1](https://github.com/SergioPerezLoza/p4_interfaces_inteligentes/blob/main/My-project-4-SampleScene-Windows_-Mac_-Linux-Unity-2021.3.gif)

```csharp
using UnityEngine;
using System;

public class ejer1_cilindro : MonoBehaviour
{
    // Evento que se dispara cuando el cubo colisiona con el cilindro
    public static event Action OnCuboColisionaConCilindro;

    // Detecta la colisión con el cubo
    private void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.CompareTag("Cubo"))
        {
            Debug.Log("Cubo ha colisionado con el Cilindro");

            // Dispara el evento para que las esferas reaccionen
            if (OnCuboColisionaConCilindro != null)
            {
                OnCuboColisionaConCilindro.Invoke();
            }
        }
    }
}

```

```csharp
using UnityEngine;

public class ejer1_cubo : MonoBehaviour
{
    public float velocidad = 5f; // Velocidad de movimiento

    void Update()
    {
        // Obtener entrada del teclado
        float inputVertical = 0f;

        // Tecla para mover hacia arriba
        if (Input.GetKey(KeyCode.U))
        {
            inputVertical = 1f; // Subir
        }
        // Tecla para mover hacia abajo
        else if (Input.GetKey(KeyCode.J))
        {
            inputVertical = -1f; // Bajar
        }

        // Mover el cubo hacia arriba o abajo
        transform.Translate(0, inputVertical * velocidad * Time.deltaTime, 0);
    }
}

```

```csharp
using UnityEngine;

public class ejer1_esfera1 : MonoBehaviour
{
    public Transform objetivoEsferaTipo2; // Esfera de tipo 2 hacia la que se moverá
    public float velocidad = 5f; // Velocidad de movimiento

    private void OnEnable()
    {
        // Suscribirse al evento
        ejer1_cilindro.OnCuboColisionaConCilindro += MoverHaciaObjetivo;
    }

    private void OnDisable()
    {
        // Desuscribirse del evento
        ejer1_cilindro.OnCuboColisionaConCilindro -= MoverHaciaObjetivo;
    }

    // Método que se llama cuando ocurre la colisión
    void MoverHaciaObjetivo()
    {
        // Iniciar el movimiento hacia la esfera de tipo 2
        StartCoroutine(MoverHaciaEsferaTipo2());
    }

    System.Collections.IEnumerator MoverHaciaEsferaTipo2()
    {
        while (Vector3.Distance(transform.position, objetivoEsferaTipo2.position) > 0.1f)
        {
            // Moverse hacia la esfera de tipo 2
            transform.position = Vector3.MoveTowards(transform.position, objetivoEsferaTipo2.position, velocidad * Time.deltaTime);
            yield return null;
        }
    }
}

```

```csharp
using UnityEngine;

public class ejer1_esfera2 : MonoBehaviour
{
    public Transform cilindro; // Cilindro hacia el que se moverá
    public float velocidad = 5f; // Velocidad de movimiento

    private void OnEnable()
    {
        // Suscribirse al evento
        ejer1_cilindro.OnCuboColisionaConCilindro += MoverHaciaCilindro;
    }

    private void OnDisable()
    {
        // Desuscribirse del evento
        ejer1_cilindro.OnCuboColisionaConCilindro -= MoverHaciaCilindro;
    }

    // Método que se llama cuando ocurre la colisión
    void MoverHaciaCilindro()
    {
        // Iniciar el movimiento hacia el cilindro
        StartCoroutine(MoverHaciaElCilindro());
    }

    System.Collections.IEnumerator MoverHaciaElCilindro()
    {
        while (Vector3.Distance(transform.position, cilindro.position) > 0.1f)
        {
            // Moverse hacia el cilindro
            transform.position = Vector3.MoveTowards(transform.position, cilindro.position, velocidad * Time.deltaTime);
            yield return null;
        }
    }
}

```


**Ejercicio 2**:
Para el ejercicio 2 hemos sustituido con el asset importado las esferas por arañas y el cilindro por huevos de arañas.
![image](https://github.com/user-attachments/assets/e5a14f91-427c-4cd8-854a-08c7e0a0459d)

**Ejercicio 3**:
![video](https://github.com/SergioPerezLoza/p4_interfaces_inteligentes/blob/main/My-project-_2_-Untitled-Windows_-Mac_-Linux-Unity-6-_6000.0.23f1__-_DX11_-2024-10-23-19-15-44.gif)
**Ejercicio 4**:
![video](https://github.com/SergioPerezLoza/p4_interfaces_inteligentes/blob/main/My-project-_2_-Untitled-Windows_-Mac_-Linux-Unity-6-_6000.0.23f1__-_DX11_-2024-10-23-19-59-48.gif)
**Ejercicio 5**:
![video](https://github.com/SergioPerezLoza/p4_interfaces_inteligentes/blob/main/My-project-_2_-Untitled-Windows_-Mac_-Linux-Unity-6-_6000.0.23f1__-_DX11_-2024-10-24-18-15-32.gif)
**Ejercicio 6**:
![video](https://github.com/SergioPerezLoza/p4_interfaces_inteligentes/blob/main/My-project-_2_-Untitled-Windows_-Mac_-Linux-Unity-6-_6000.0.23f1__-_DX11_-2024-10-24-19-44-20.gif)
**Ejercicio 7**:
![video]()
**Ejercicio 8**:
![video]()
**Ejercicio 9**:
Hacer objeto físico el cubo ya fue implementado en el ejercicio 3 así que es lo mismo.
