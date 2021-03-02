using System.Collections;

using UnityEngine;

public class ShowMainMenuButton : MonoBehaviour
{

    public static bool readyWork;

    private SpriteRenderer button1Renderer,
                           button2Renderer,
                           button3Renderer,
                           button4Renderer,
                           button5Renderer;

    private Color colorButton1,
                  colorButton2,
                  colorButton3,
                  colorButton4,
                  colorButton5;

    private float speedButton, speedAlphaChange, speedVanishing;

    private Vector2 finalPos1,
                    finalPos2,
                    finalPos3,
                    finalPos4;

    private bool showButt,
                 showButt1,
                 showButt2,
                 showButt3,
                 showButt4,
                 showButt5;

    private float colorFade1,
                  colorFade2,
                  colorFade3,
                  colorFade4,
                  colorFade5;

    ////////////////////////////////////////////////////////////////////////////////////////////////////////////
                                                                                                               //
    void Start ()                                                                                              // 
    {                                                                                                          // 
        button1Renderer = transform.GetChild(0).transform.GetChild(0).transform.GetComponent<SpriteRenderer>(); //
        button2Renderer = transform.GetChild(1).transform.GetChild(0).transform.GetComponent<SpriteRenderer>(); //
        button3Renderer = transform.GetChild(2).transform.GetChild(0).transform.GetComponent<SpriteRenderer>(); //
        button4Renderer = transform.GetChild(3).transform.GetChild(0).transform.GetComponent<SpriteRenderer>(); //
        button5Renderer = transform.GetChild(4).transform.GetChild(0).transform.GetComponent<SpriteRenderer>(); //
                                                                                                               // 
        colorButton1 = button1Renderer.color;                                                                  // 
        colorButton2 = button2Renderer.color;                                                                  //     
        colorButton3 = button3Renderer.color;                                                                  // 
        colorButton4 = button4Renderer.color;                                                                  //
        colorButton5 = button5Renderer.color;                                                                  // 
                                                                                                               //
        colorFade1 = button1Renderer.material.GetFloat("_Fade");                                               //
        colorFade2 = button2Renderer.material.GetFloat("_Fade");                                               //
        colorFade3 = button3Renderer.material.GetFloat("_Fade");                                               //         
        colorFade4 = button4Renderer.material.GetFloat("_Fade");                                               //
        colorFade5 = button5Renderer.material.GetFloat("_Fade");                                               // 
                                                                                                               // 
        finalPos1 = new Vector2(0.242f, -1.354f);                                                              //     
        finalPos2 = new Vector2(-0.891f, -1.097f);                                                             // 
        finalPos3 = new Vector2(-1.422f, -0.079f);                                                             // 
        finalPos4 = new Vector2(-1.031f, 0.95f);                                                               //     
                                                                                                               // 
        speedButton = 3f * Time.fixedDeltaTime;                                                                // 
        speedAlphaChange = 1f * Time.fixedDeltaTime;                                                           //
        speedVanishing = 1.4f * Time.fixedDeltaTime;                                                           //     
                                                                                                               // 
        showButt = false;                                                                                      //     
        showButt1 = false;                                                                                     //
        showButt2 = false;                                                                                     //
        showButt3 = false;                                                                                     //     
        showButt4 = false;                                                                                     //
        showButt5 = false;                                                                                     //
                                                                                                               //
        readyWork = false;                                                                                     //
                                                                                                               //
        for (int i = 0; i < transform.childCount; i++)                                                         //
        {                                                                                                      //
            transform.GetChild(i).transform.GetChild(1).transform.                                             //     
                GetComponent<SpriteRenderer>().material.SetFloat("_Fade", 1f);                                 //
        }                                                                                                      // 
    }                                                                                                          //     
                                                                                                               // 
    ////////////////////////////////////////////////////////////////////////////////////////////////////////////
    
                                                                                                               
    void OnEnable ()                                                                                           
    {                                                                                                          
        showButt = true;                                                                                       
                                                                                                               
        StartCoroutine(timeControll());                                                                        
    }                                                                                                          

    ////////////////////////////////////////////////////////////////////////////////////////////////////////////

    void FixedUpdate()
    {

        if (showButt1)
        {
            if (colorFade1 < 1f)
            {
                colorFade1 += speedVanishing;
                button1Renderer.material.SetFloat("_Fade", colorFade1);
            }

            if (transform.GetChild(0).transform.localPosition.x < finalPos1.x && transform.GetChild(0).transform.localPosition.y > finalPos1.y)
                transform.GetChild(0).transform.Translate(speedButton * Mathf.Abs(finalPos1.x / finalPos1.y), -speedButton, 0);
        }
        if (showButt2)
        {
            if (colorFade2 < 1f)
            {
                colorFade2 += speedVanishing;
                button2Renderer.material.SetFloat("_Fade", colorFade2);
            }

            if (transform.GetChild(1).transform.localPosition.x > finalPos2.x && transform.GetChild(1).transform.localPosition.y > finalPos2.y)
                transform.GetChild(1).transform.Translate(-speedButton * Mathf.Abs(finalPos2.x / finalPos2.y), -speedButton, 0);
        }
        if (showButt3)
        {

            if (colorFade3 < 1f)
            {
                colorFade3 += speedVanishing;
                button3Renderer.material.SetFloat("_Fade", colorFade3);
            }

            if (transform.GetChild(2).transform.localPosition.x > finalPos3.x && transform.GetChild(2).transform.localPosition.y > finalPos3.y)
                transform.GetChild(2).transform.Translate(-speedButton, -speedButton * Mathf.Abs(finalPos3.y / finalPos3.x), 0);
        }
        if (showButt4)
        {

            if (colorFade4 < 1f)
            {
                colorFade4 += speedVanishing;
                button4Renderer.material.SetFloat("_Fade", colorFade4);
            }

            if (transform.GetChild(3).transform.localPosition.x > finalPos4.x && transform.GetChild(3).transform.localPosition.y < finalPos4.y)
                transform.GetChild(3).transform.Translate(-speedButton, speedButton * Mathf.Abs(finalPos4.y / finalPos4.x), 0);
        }
        if (showButt5)
        {

            if (colorFade5 < 1f)
            {
                colorFade5 += speedVanishing;
                button5Renderer.material.SetFloat("_Fade", colorFade5);
            } else
            {
                if (!readyWork)
                {
                    ShakingCamera.shakeCamera();
                    PortalControll.deletePortal();

                    readyWork = true;
                }
            }
        }






    }
    
    ////////////////////////////////////////////////////////////////////////////////////////////////////////////

    IEnumerator timeControll ()
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
