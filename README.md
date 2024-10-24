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
```csharp
using UnityEngine;
using System.Collections;

public class ejer2_araña1 : MonoBehaviour
{
    public Transform objetivoHuevoTipo2; // El huevo tipo 2 hacia el que se moverá la araña
    public float velocidad = 5f; // Velocidad de movimiento

    private void OnEnable()
    {
        // Suscribirse al evento de colisión del cubo con una araña de tipo 1
        ejer2_cubo.OnCuboColisionaConArañaTipo1 += MoverHaciaHuevoTipo2;
    }

    private void OnDisable()
    {
        // Desuscribirse del evento
        ejer2_cubo.OnCuboColisionaConArañaTipo1 -= MoverHaciaHuevoTipo2;
    }

    // Método que se llama cuando el cubo colisiona con una araña tipo 1
    void MoverHaciaHuevoTipo2()
    {
        StartCoroutine(MoverHaciaHuevo());
    }

    // Corrutina para mover la araña hacia el huevo
    IEnumerator MoverHaciaHuevo()
    {
        while (Vector3.Distance(transform.position, objetivoHuevoTipo2.position) > 0.1f)
        {
            transform.position = Vector3.MoveTowards(transform.position, objetivoHuevoTipo2.position, velocidad * Time.deltaTime);
            yield return null;
        }
    }

    // Detectar colisión con el huevo
    private void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.CompareTag("HuevoTipo2"))
        {
            Debug.Log("Araña de tipo 1 colisionó con un Huevo tipo 2, cambiando color");
            GetComponent<Renderer>().material.color = Color.red; // Cambia el color de la araña al colisionar
        }
    }
}

```
```csharp
using UnityEngine;
using System.Collections;

public class ejer2_araña2 : MonoBehaviour
{
    public Transform objetivo; // El objeto hacia el que se moverán las arañas de tipo 1
    public float velocidad = 5f; // Velocidad de movimiento

    private void OnEnable()
    {
        // Suscribirse al evento de colisión del cubo con una araña de tipo 2
        ejer2_cubo.OnCuboColisionaConArañaTipo2 += MoverHaciaObjetivo;
    }

    private void OnDisable()
    {
        // Desuscribirse del evento
        ejer2_cubo.OnCuboColisionaConArañaTipo2 -= MoverHaciaObjetivo;
    }

    // Método que se llama cuando el cubo colisiona con una araña tipo 2
    void MoverHaciaObjetivo()
    {
        StartCoroutine(MoverHaciaObjeto());
    }

    // Corrutina para mover la araña hacia el objeto
    IEnumerator MoverHaciaObjeto()
    {
        while (Vector3.Distance(transform.position, objetivo.position) > 0.1f)
        {
            transform.position = Vector3.MoveTowards(transform.position, objetivo.position, velocidad * Time.deltaTime);
            yield return null;
        }
    }
}

```
```csharp
using UnityEngine;
using System;

public class ejer2_cubo : MonoBehaviour
{
    public static event Action OnCuboColisionaConArañaTipo1;
    public static event Action OnCuboColisionaConArañaTipo2;

    private void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.CompareTag("ArañaTipo1"))
        {
            Debug.Log("Cubo colisionó con Araña tipo 1");
            OnCuboColisionaConArañaTipo1?.Invoke();
        }
        else if (collision.gameObject.CompareTag("ArañaTipo2"))
        {
            Debug.Log("Cubo colisionó con Araña tipo 2");
            OnCuboColisionaConArañaTipo2?.Invoke();
        }
    }
}

```
**Ejercicio 4**:
![video](https://github.com/SergioPerezLoza/p4_interfaces_inteligentes/blob/main/My-project-_2_-Untitled-Windows_-Mac_-Linux-Unity-6-_6000.0.23f1__-_DX11_-2024-10-23-19-59-48.gif)
```csharp
using UnityEngine;

public class ejer3_araña1 : MonoBehaviour
{
    public Transform huevoObjetivo; // Huevo al que se teletransportarán
    public bool teletransportada = false; // Para evitar múltiples teletransportaciones

    private void OnEnable()
    {
        // Suscribirse al evento de proximidad del cubo
        ejer3_cubo.OnCuboSeAproximaAlObjeto += TeletransportarAlaPosicionDelHuevo;
    }

    private void OnDisable()
    {
        // Desuscribirse del evento
        ejer3_cubo.OnCuboSeAproximaAlObjeto -= TeletransportarAlaPosicionDelHuevo;
    }

    // Teletransportar la araña a la posición del huevo objetivo
    void TeletransportarAlaPosicionDelHuevo()
    {
        if (!teletransportada)
        {
            transform.position = huevoObjetivo.position;
            teletransportada = true; // Para asegurarse de que solo se teletransporte una vez
            Debug.Log("Araña de tipo 1 teletransportada al huevo.");
        }
    }
}

```
```csharp
using UnityEngine;

public class ejer3_araña2 : MonoBehaviour
{
    public Transform objetoObjetivo; // El objeto hacia el que se orientarán las arañas
    public Transform huevoObjetivo; // Opcional: Huevo al que se teletransportarán si lo deseas
    public bool teletransportada = false; // Para evitar múltiples teletransportaciones

    private void OnEnable()
    {
        // Suscribirse al evento de proximidad del cubo
        ejer3_cubo.OnCuboSeAproximaAlObjeto += TeletransportarOOrientar;
    }

    private void OnDisable()
    {
        // Desuscribirse del evento
        ejer3_cubo.OnCuboSeAproximaAlObjeto -= TeletransportarOOrientar;
    }

    void TeletransportarOOrientar()
    {
        // Si no se ha teletransportado y se tiene un huevo objetivo, teletransportar
        if (!teletransportada && huevoObjetivo != null)
        {
            transform.position = huevoObjetivo.position;
            teletransportada = true; // Asegurarse de que solo se teletransporte una vez
            Debug.Log("Araña de tipo 2 teletransportada al huevo.");
        }
        else
        {
            // Si ya se ha teletransportado, se orientará hacia el objeto objetivo
            OrientarHaciaObjeto();
        }
    }

    void OrientarHaciaObjeto()
    {
        // Dirección hacia el objeto objetivo
        Vector3 direccion = objetoObjetivo.position - transform.position;
        direccion.y = 0; // Opcional: Para mantener la orientación solo en el eje horizontal

        // Rotar la araña hacia el objeto objetivo
        if (direccion != Vector3.zero)
        {
            Quaternion rotacion = Quaternion.LookRotation(direccion);
            transform.rotation = Quaternion.Slerp(transform.rotation, rotacion, Time.deltaTime * 2f); // Rotación suave
        }
    }

    void Update()
    {
        // Solo se orienta si no ha sido teletransportada
        if (!teletransportada)
        {
            OrientarHaciaObjeto();
        }
    }
}


```
```csharp
using UnityEngine;
using System;

public class ejer3_cubo : MonoBehaviour
{
    public Transform objetoDeReferencia; // El objeto al que el cubo se aproxima
    public float distanciaActivacion = 5f; // Distancia a la que se activará el evento
    public static event Action OnCuboSeAproximaAlObjeto; // Evento que activa el teletransporte

    void Update()
    {
        // Calculamos la distancia entre el cubo y el objeto de referencia
        float distancia = Vector3.Distance(transform.position, objetoDeReferencia.position);

        // Si la distancia es menor a la distancia de activación, emitimos el evento
        if (distancia <= distanciaActivacion)
        {
            OnCuboSeAproximaAlObjeto?.Invoke();
        }
    }
}

```
**Ejercicio 5**:
![video](https://github.com/SergioPerezLoza/p4_interfaces_inteligentes/blob/main/My-project-_2_-Untitled-Windows_-Mac_-Linux-Unity-6-_6000.0.23f1__-_DX11_-2024-10-24-18-15-32.gif)
```csharp
using UnityEngine;

public class ejer5_cubo : MonoBehaviour
{
    private int puntuacion = 0; // Puntuación del jugador

    // Método para gestionar colisiones
    private void OnTriggerEnter(Collider other)
    {
        // Verifica si el cubo ha chocado con un objeto que tiene el script "Huevo"
        ejer5_huevo huevo = other.gameObject.GetComponent<ejer5_huevo>();

        if (huevo != null) // Si el objeto es un huevo
        {
            // Sumar puntos según el tipo de araña asociado al huevo
            if (huevo.tipoAraña == "ArañaTipo1")
            {
                puntuacion += 50;
                Debug.Log("Araña tipo 1 recolectada. Puntuación actual: " + puntuacion);
            }
            else if (huevo.tipoAraña == "ArañaTipo2")
            {
                puntuacion += 100;
                Debug.Log("Araña tipo 2 recolectada. Puntuación actual: " + puntuacion);
            }

            // Destruir el huevo después de recogerlo
            Destroy(other.gameObject);
        }
    }
}

```
```csharp
using UnityEngine;

public class ejer5_huevo : MonoBehaviour
{
    // Definir si el huevo pertenece a una araña de tipo 1 o 2
    public string tipoAraña; // "Araña1" o "Araña2"

    private void Start()
    {
        // Asegurarse de que el huevo tenga un Collider con IsTrigger activado para detectar la colisión
        Collider collider = GetComponent<Collider>();
        if (collider != null && !collider.isTrigger)
        {
            collider.isTrigger = true;
        }
    }
}

// probando...
```
**Ejercicio 6**:
![video](https://github.com/SergioPerezLoza/p4_interfaces_inteligentes/blob/main/My-project-_2_-Untitled-Windows_-Mac_-Linux-Unity-6-_6000.0.23f1__-_DX11_-2024-10-24-19-44-20.gif)
```csharp
  using UnityEngine;
  using UnityEngine.UI;  // Importante para acceder a la UI

  public class ejer6_cubo : MonoBehaviour
  {
      private int puntuacion = 0; // Puntuación del jugador
      public Text textoPuntuacion; // Referencia al componente de texto de la UI

      // Método para gestionar colisiones
      private void OnTriggerEnter(Collider other)
      {
          // Verifica si el cubo ha chocado con un objeto que tiene el script "Huevo"
          ejer5_huevo huevo = other.gameObject.GetComponent<ejer5_huevo>();

          if (huevo != null) // Si el objeto es un huevo
          {
              // Sumar puntos según el tipo de araña asociado al huevo
              if (huevo.tipoAraña == "ArañaTipo1")
              {
                  puntuacion += 5;
                  Debug.Log("Araña tipo 1 recolectada. Puntuación actual: " + puntuacion);
              }
              else if (huevo.tipoAraña == "ArañaTipo2")
              {
                  puntuacion += 10;
                  Debug.Log("Araña tipo 2 recolectada. Puntuación actual: " + puntuacion);
              }

              // Actualizar el texto de la puntuación en la interfaz
              textoPuntuacion.text = "Puntuación: " + puntuacion;

              // Destruir el huevo después de recogerlo
              Destroy(other.gameObject);
          }
      }
  }



```
**Ejercicio 7**:
![video]()
**Ejercicio 8**:
![video]()
**Ejercicio 9**:
Hacer objeto físico el cubo ya fue implementado en el ejercicio 3 así que es lo mismo.
