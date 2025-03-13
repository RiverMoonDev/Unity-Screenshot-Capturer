using UnityEngine;
using System.IO;
using TMPro;
using System.Collections;

public class ScreenshotCapture : MonoBehaviour
{
    public KeyCode screenshotKey = KeyCode.F12; // Default key
    public string gameName = "Placeholder"; // Change this to your game's name
    public CanvasGroup flashEffect; //Flash Effect (CanvasGroup required)
    public TextMeshProUGUI notificationText; // UI Text for notifications
    public AudioSource shutterSound; // Sound effect
    private string savePath;

    void Start()
    {
        string picturesPath = System.Environment.GetFolderPath(System.Environment.SpecialFolder.MyPictures);
        savePath = Path.Combine(picturesPath, gameName, "Screenshots");

        if (!Directory.Exists(savePath))
        {
            Directory.CreateDirectory(savePath);
        }

        Debug.Log("Screenshots will be saved in: " + savePath);
    }

    void Update()
    {
        if (Input.GetKeyDown(screenshotKey))
        {
            StartCoroutine(CaptureScreenshot());
        }
    }

    IEnumerator CaptureScreenshot()
    {
        if (notificationText) notificationText.gameObject.SetActive(false);
        if (flashEffect) flashEffect.gameObject.SetActive(false);

        yield return new WaitForEndOfFrame();

        string fileName = "screenshot_" + System.DateTime.Now.ToString("yyyy-MM-dd_HH-mm-ss") + ".png";
        string filePath = Path.Combine(savePath, fileName);

        ScreenCapture.CaptureScreenshot(filePath);
        Debug.Log("Screenshot saved: " + filePath);

        yield return new WaitForSeconds(0.2f);

        if (shutterSound) shutterSound.Play();

        StartCoroutine(ShowFlashEffect());

        ShowNotification("Screenshot saved!");
    }

    IEnumerator ShowFlashEffect()
    {
        if (flashEffect)
        {
            flashEffect.gameObject.SetActive(true);
            flashEffect.alpha = 1;
            yield return new WaitForSeconds(0.1f);
            while (flashEffect.alpha > 0)
            {
                flashEffect.alpha -= Time.deltaTime * 3;
                yield return null;
            }
            flashEffect.gameObject.SetActive(false);
        }
    }

    void ShowNotification(string message)
    {
        if (notificationText)
        {
            notificationText.text = message;
            notificationText.gameObject.SetActive(true);
            CancelInvoke("HideNotification");
            Invoke("HideNotification", 2f);
        }
    }

    void HideNotification()
    {
        if (notificationText) notificationText.gameObject.SetActive(false);
    }
}
