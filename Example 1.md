
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public struct MainButton
{
    public SpriteRenderer buttonSprite;
    public Color buttonColor;
    public Vector2 finalPos;
    public float colorFade;
}

public class ShowMainButtonTest : MonoBehaviour
{
    public static bool readyWork;

    private float speedButton, speedAlphaChange, speedVanishing;

    private bool showButt,
                 showButt1,
                 showButt2,
                 showButt3,
                 showButt4,
                 showButt5;

    private MainButton[] buttons = new MainButton[5];

    public SpriteRenderer test;

    ////////////////////////////////////////////////////////////////////////////////////////////////////////////
                                                                                                              //
    void Start()                                                                                              // 
    {                                                                                                         //
                                                                                                              //
        for (int i = 0; i < buttons.Length; i++)                                                              //
        {                                                                                                     //
            buttons[i].buttonSprite = transform.GetChild(i).GetChild(0).GetComponent<SpriteRenderer>();       //
            buttons[i].buttonColor = buttons[i].buttonSprite.color;                                           //
            buttons[i].colorFade = buttons[i].buttonSprite.material.GetFloat("_Fade");                        //
                                                                                                              //
            switch (i) {                                                                                      //
                case 0 :                                                                                      //
                    buttons[i].finalPos = new Vector2(0.242f, -1.354f);                                       //
                    break;                                                                                    //
                case 1:                                                                                       //
                    buttons[i].finalPos = new Vector2(-0.891f, -1.097f);                                      //
                    break;                                                                                    //
                case 2:                                                                                       //
                    buttons[i].finalPos = new Vector2(-1.422f, -0.079f);                                      //
                    break;                                                                                    //
                case 3:                                                                                       //
                    buttons[i].finalPos = new Vector2(-1.031f, 0.95f);                                        //
                    break;                                                                                    //
                case 4:                                                                                       //
                    break;                                                                                    //
                                                                                                              //
            }                                                                                                 //
        }                                                                                                     //
                                                                                                              // 
        speedButton = 3f * Time.fixedDeltaTime;                                                               // 
        speedAlphaChange = 1f * Time.fixedDeltaTime;                                                          //
        speedVanishing = 1.4f * Time.fixedDeltaTime;                                                          //     
                                                                                                              // 
        showButt = false;                                                                                     //     
        showButt1 = false;                                                                                    //
        showButt2 = false;                                                                                    //
        showButt3 = false;                                                                                    //     
        showButt4 = false;                                                                                    //
        showButt5 = false;                                                                                    //
                                                                                                              //
        readyWork = false;                                                                                    //
                                                                                                              //
        for (int i = 0; i < transform.childCount; i++)                                                        //
        {                                                                                                     //
            transform.GetChild(i).transform.GetChild(1).transform.                                            //     
                GetComponent<SpriteRenderer>().material.SetFloat("_Fade", 1f);                                //
        }                                                                                                     // 
    }                                                                                                         //     
                                                                                                              // 
    ////////////////////////////////////////////////////////////////////////////////////////////////////////////
   
    void OnEnable()                                                                                         
    {                                                                                                         
        showButt = true;                                                                                      
                                                                                                               
        StartCoroutine(timeControll());                                                                        
    }                                                                                                          

    ////////////////////////////////////////////////////////////////////////////////////////////////////////////

    void FixedUpdate()
    {

        if (showButt1)
        {
            MoveButton(0);
        }
        if (showButt2)
        {
            MoveButton(1);
        }
        if (showButt3)
        {
            MoveButton(2);
        }
        if (showButt4)
        {
            MoveButton(3);
        }
        if (showButt5)
        {
            MoveButton(4);
        }

    }

    ////////////////////////////////////////////////////////////////////////////////////////////////////////////
    
    void MoveButton (int index)
    {
        switch (index)
        {
            case 0 :
                if (buttons[index].colorFade < 1f)
                {
                    buttons[index].colorFade += speedVanishing;
                    buttons[index].buttonSprite.material.SetFloat("_Fade", buttons[index].colorFade);
                }

                if (transform.GetChild(0).transform.localPosition.x < buttons[index].finalPos.x && transform.GetChild(0).transform.localPosition.y > buttons[index].finalPos.y)
                    transform.GetChild(0).transform.Translate(speedButton * Mathf.Abs(buttons[index].finalPos.x / buttons[index].finalPos.y), -speedButton, 0);
                break;
            case 1:
                if (buttons[index].colorFade < 1f)
                {
                    buttons[index].colorFade += speedVanishing;
                    buttons[index].buttonSprite.material.SetFloat("_Fade", buttons[index].colorFade);
                }

                if (transform.GetChild(1).transform.localPosition.x > buttons[index].finalPos.x && transform.GetChild(1).transform.localPosition.y > buttons[index].finalPos.y)
                    transform.GetChild(1).transform.Translate(-speedButton * Mathf.Abs(buttons[index].finalPos.x / buttons[index].finalPos.y), -speedButton, 0);
                break;
            case 2:
                if (buttons[index].colorFade < 1f)
                {
                    buttons[index].colorFade += speedVanishing;
                    buttons[index].buttonSprite.material.SetFloat("_Fade", buttons[index].colorFade);
                }

                if (transform.GetChild(2).transform.localPosition.x > buttons[index].finalPos.x && transform.GetChild(2).transform.localPosition.y > buttons[index].finalPos.y)
                    transform.GetChild(2).transform.Translate(-speedButton, -speedButton * Mathf.Abs(buttons[index].finalPos.y / buttons[index].finalPos.x), 0);
                break;
            case 3:
                if (buttons[index].colorFade < 1f)
                {
                    buttons[index].colorFade += speedVanishing;
                    buttons[index].buttonSprite.material.SetFloat("_Fade", buttons[index].colorFade);
                }

                if (transform.GetChild(3).transform.localPosition.x > buttons[index].finalPos.x && transform.GetChild(3).transform.localPosition.y < buttons[index].finalPos.y)
                    transform.GetChild(3).transform.Translate(-speedButton, speedButton * Mathf.Abs(buttons[index].finalPos.y / buttons[index].finalPos.x), 0);
                break;
            case 4:
                if (buttons[index].colorFade < 1f)
                {
                    buttons[index].colorFade += speedVanishing;
                    buttons[index].buttonSprite.material.SetFloat("_Fade", buttons[index].colorFade);
                }
                else
                {
                    if (!readyWork)
                    {
                        ShakingCamera.shakeCamera();
                        PortalControll.deletePortal();

                        readyWork = true;
                    }
                }
                break;
        }
    }

    IEnumerator timeControll()
    {
        showButt = true;
        yield return new WaitForSeconds(2f);
        showButt1 = true;
        yield return new WaitForSeconds(0.5f);
        showButt2 = true;
        yield return new WaitForSeconds(0.5f);
        showButt3 = true;
        yield return new WaitForSeconds(0.5f);
        showButt4 = true;
        yield return new WaitForSeconds(0.5f);
        showButt5 = true;
    }

}
