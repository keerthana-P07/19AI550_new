# Ex.No: 10  Implementation of 2D/3D game -------------------
### DATE: 08-09-2026                                                                            
### REGISTER NUMBER : 212225230138
### AIM: 
To develop a game in Unity 
### Algorithm:
```
Step 1: Start the game.

Step 2: Load the game environment:

* Forest background
* Ground/platform
* Player character
* Obstacles/enemies
* Score and High Score UI

Step 3: Display the Start Screen with:

* Score: 0
* PRESS SPACE TO START
* High Score

Step 4: Wait for the player to press the Spacebar.

Step 5: When Spacebar is pressed:

* Start the game.
* Start player movement.
* Begin obstacle generation.
* Start score counting.

Step 6: Continuously move the player forward automatically.

Step 7: Check keyboard input:

* If Spacebar is pressed → make the player jump.
* Otherwise → keep the player running on the ground.

Step 8: Continuously generate/move obstacles toward the player.

Step 9: Detect collisions:

* If the player hits an obstacle → Game Over.
* If the player successfully passes an obstacle → increase the score.

Step 10: Continuously update the score based on the player’s survival/distance.

Step 11: When Game Over occurs:

* Stop player movement.
* Stop obstacle movement.
* Display the final score.
* Compare the score with the previous High Score.

Step 12: If the current score is greater than the High Score:

* Update the High Score.

Step 13: Display an option/message to restart the game.

Step 14: If the player presses Spacebar/Restart:

* Reset the score.
* Reset the player position.
* Remove/reset obstacles.
* Start the game again.

Step 15: Repeat Steps 5–14 until the player exits the game.
```  
### Program:
```
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    public float moveSpeed = 5f;
    public float jumpForce = 12f;

    private Rigidbody2D rb;
    private bool isGrounded;

    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        // Automatic forward movement
        rb.velocity = new Vector2(moveSpeed, rb.velocity.y);

        // Jump
        if (Input.GetKeyDown(KeyCode.Space) && isGrounded)
        {
            rb.velocity = new Vector2(rb.velocity.x, jumpForce);
            isGrounded = false;
        }
    }

    private void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.gameObject.CompareTag("Ground"))
        {
            isGrounded = true;
        }

        if (collision.gameObject.CompareTag("Obstacle"))
        {
            GameManager.instance.GameOver();
        }
    }
}

using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;

public class GameManager : MonoBehaviour
{
    public static GameManager instance;

    public Text scoreText;
    public Text highScoreText;
    public GameObject startScreen;
    public GameObject gameOverScreen;

    private int score = 0;
    private int highScore = 0;
    private bool gameStarted = false;

    void Awake()
    {
        instance = this;
    }

    void Start()
    {
        highScore = PlayerPrefs.GetInt("HighScore", 0);

        scoreText.text = "Score: 0";
        highScoreText.text = "High Score: " + highScore;

        Time.timeScale = 0;
    }

    void Update()
    {
        if (!gameStarted && Input.GetKeyDown(KeyCode.Space))
        {
            StartGame();
        }
    }

    void StartGame()
    {
        gameStarted = true;
        startScreen.SetActive(false);
        Time.timeScale = 1;

        InvokeRepeating("IncreaseScore", 1f, 1f);
    }

    void IncreaseScore()
    {
        score++;
        scoreText.text = "Score: " + score;
    }

    public void GameOver()
    {
        Time.timeScale = 0;

        if (score > highScore)
        {
            highScore = score;
            PlayerPrefs.SetInt("HighScore", highScore);
        }

        highScoreText.text = "High Score: " + highScore;
        gameOverScreen.SetActive(true);
    }

    public void RestartGame()
    {
        Time.timeScale = 1;
        SceneManager.LoadScene(SceneManager.GetActiveScene().buildIndex);
    }
}

using UnityEngine;

public class ObstacleSpawner : MonoBehaviour
{
    public GameObject obstaclePrefab;
    public float spawnTime = 2f;
    public float spawnDistance = 15f;

    void Start()
    {
        InvokeRepeating("SpawnObstacle", 2f, spawnTime);
    }

    void SpawnObstacle()
    {
        Vector3 position = transform.position;
        position.x += spawnDistance;

        Instantiate(obstaclePrefab, position, Quaternion.identity);
    }
}

using UnityEngine;

public class ObstacleMovement : MonoBehaviour
{
    public float speed = 5f;

    void Update()
    {
        transform.Translate(Vector2.left * speed * Time.deltaTime);

        if (transform.position.x < -20f)
        {
            Destroy(gameObject);
        }
    }
}

using UnityEngine;

public class GameStart : MonoBehaviour
{
    public GameObject startPanel;

    void Start()
    {
        Time.timeScale = 0;
    }

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            startPanel.SetActive(false);
            Time.timeScale = 1;
        }
    }
}





```
### Output:
<img width="1280" height="853" alt="WhatsApp Image 2026-09-08 at 8 26 40 PM" src="https://github.com/user-attachments/assets/c5ab833d-7308-4914-b74e-2a898eac2bcc" />



### Result:
Thus the game was developed using Unity and adopted AI technology.
