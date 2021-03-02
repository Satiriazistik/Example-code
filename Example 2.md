using System.Collections;
using UnityEngine;

public class MoveStealingLight : MonoBehaviour
{
    private float speed, biasX, biasY;
    private float x, y;

    private bool finishX, finishY, onPos, deleteWhenOnPos;

    private ParticleSystem stealingLigtSys;

    private float divisionCoef;

    private int collectedLigt;

    private GameObject defultParent;

    private void Awake()
    {
        if (transform.parent.parent.parent.gameObject.name != "Back_Ground_Base(Clone)")
        {
            deleteWhenOnPos = true;
        }
    }

    void OnEnable()
    {
        speed = 4f * Time.fixedDeltaTime; //5 оптимально быстрая
        finishX = false;
        finishY = false;
        onPos = false;

        defultParent = transform.parent.gameObject;

        stealingLigtSys = GetComponent<ParticleSystem>();

        collectedLigt = Random.Range(1, 11);

        if (collectedLigt < 5)
        {
            collectedLigt = 1;
        }
        else if (collectedLigt >= 5 && collectedLigt < 8)
        {
            collectedLigt = 2;
        }
        else if (collectedLigt >= 8 && collectedLigt < 10)
        {
            collectedLigt = 3;
        }
        else if (collectedLigt == 10)
        {
            collectedLigt = 4;
        }

        if (_GameControll.lose)
        {
            gameObject.SetActive(false);
        }

    }

    void FixedUpdate()
    {
        x = -transform.localPosition.x; //Если игрок правее похищенного света, то x - положительный.
        y = -transform.localPosition.y; //Если игрок выше похищенного света, то y - положительный.

        if (Mathf.Abs(x) >= Mathf.Abs(y) && !onPos)
        {
            divisionCoef = Mathf.Abs(y / x);

            //Если игрок находится правее похищенного света
            if (x > 0f)
            {
                transform.Translate(speed - PlayerControll.deltaXGlob, 0, 0);
            }
            //Если игрок находится левее похищенного света
            if (x < 0f)
            {
                transform.Translate(-speed - PlayerControll.deltaXGlob, 0, 0);
            }
            //Если игрок находится выше похищенного света
            if (y > 0f)
            {
                transform.Translate(0, speed * divisionCoef - PlayerControll.deltaYGlob, 0);

            }
            //Если игрок находится ниже похищенного света
            if (y < 0f)
            {
                transform.Translate(0, -speed * divisionCoef - PlayerControll.deltaYGlob, 0);
            }

        } else if (Mathf.Abs(x) < Mathf.Abs(y) && !onPos) {
            divisionCoef = Mathf.Abs(x / y);

            //Если игрок находится правее похищенного света
            if (x > 0f)
            {
                transform.Translate(speed * divisionCoef - PlayerControll.deltaXGlob, 0, 0);
            }
            //Если игрок находится левее похищенного света
            if (x < 0f)
            {
                transform.Translate(-speed * divisionCoef - PlayerControll.deltaXGlob, 0, 0);
            }
            //Если игрок находится выше похищенного света
            if (y > 0f)
            {
                transform.Translate(0, speed - PlayerControll.deltaYGlob, 0);

            }
            //Если игрок находится ниже похищенного света
            if (y < 0f)
            {
                transform.Translate(0, -speed - PlayerControll.deltaYGlob, 0);
            }

        }

        if (transform.localPosition.x < 0.1f && transform.localPosition.x > -0.1f && transform.localPosition.y < 0.1f && transform.localPosition.y > -0.1f && !onPos)
        {
            finishX = true;
            finishY = true;
            onPos = true;
            StartCoroutine(endStealingLight());
        }

    }

    IEnumerator endStealingLight()
    {
        yield return new WaitForSeconds(0.2f);
        stealingLigtSys.Stop();
        
        yield return new WaitForSeconds(0.5f);
        LightCount.globalCollictedLight += collectedLigt;
        
        yield return new WaitForSeconds(0.2f);
        if (deleteWhenOnPos)
        {
            Destroy(gameObject);
        }
        transform.parent = defultParent.transform;
        transform.localPosition = new Vector3(0f, 0f, -1f);
        gameObject.SetActive(false);

    }

}
