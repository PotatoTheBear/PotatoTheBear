## Hi there I'm Zoë a second year software development games student👋

- 🔭 I’m currently working on a Godot Project & looking for interships.
- 🌱 I’m currently learning: C# & GDScript.
- 😄 Pronouns: she/her
- 🐻 Age: 24
- 🍔 Hobby's: I love gaming, cooking/baking, drawning, plants and working with animails.

## Over here I will showcase some code I wrote for a simple 2D platform

This is some code for a collectble item.
'''C#
public class Cheesestrings : MonoBehaviour
{
    private GameManager manager;
    // Start is called before the first frame update
    void Start()
    {
        manager = GameObject.FindGameObjectWithTag("GameController").GetComponent<GameManager>();
    }

    // Update is called once per frame
    private void OnTriggerEnter2D(Collider2D collision)
    {
        manager.AddScore(1);
        Destroy(gameObject);
    }
}
'''
This is for a enemies taking damage
'''C#
public class EnemyDamage : MonoBehaviour
{
    public int damage;
    public PlayerHealth playerHealth;

    private void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.gameObject.tag == "Player")
        {
            playerHealth.TakeDamage(damage);
        }
    }
}
'''
Here is some for enemy movement.
'''C#
public class EnemyMovement : MonoBehaviour
{
    public Transform[] patrolPoints;
    public float moveSpeed;
    public int patrolDestination;

    private void Update()
    {
        if(patrolDestination == 0)
        {
            transform.position = Vector2.MoveTowards(transform.position, patrolPoints[0].position, moveSpeed * Time.deltaTime);
            if (Vector2.Distance(transform.position, patrolPoints[0].position) < .2f)
            {
                patrolDestination = 1;
            }
        }
        if (patrolDestination == 1)
        {
            transform.position = Vector2.MoveTowards(transform.position, patrolPoints[1].position, moveSpeed * Time.deltaTime);
            if (Vector2.Distance(transform.position, patrolPoints[1].position) < .2f)
            {
                patrolDestination = 0;
            }
        }
    }
}
'''
Here is some for the player movement.
'''C#
public class PlatformMovement : MonoBehaviour
{
    [SerializeField] private float speed;
    [SerializeField] private Transform[] patrolPoints;
    [SerializeField] private float waitTime;
    int currentPointIndex;
    bool once;

    // Update is called once per frame
    void FixedUpdate()
    {
        if (transform.position != patrolPoints[currentPointIndex].position)
        {
            transform.position = Vector2.MoveTowards(transform.position,
            patrolPoints[currentPointIndex].position, speed * Time.deltaTime);
        }
        else
        {
            if (once == false)
            {
                once = true;
                StartCoroutine(Wait());
            }
        }
    }
    IEnumerator Wait()
    {
        yield return new WaitForSeconds(waitTime);
        if (currentPointIndex + 1 < patrolPoints.Length)
        {
            currentPointIndex++;
        }
            currentPointIndex = 0;
        }
        once = false;
    }
    private void OnCollisionEnter2D(Collision2D collision)
    {
        if (collision.collider.CompareTag("Player"))
        {
            collision.transform.SetParent(transform);
        }
    }
    private void OnCollisionExit2D(Collision2D collision)
    {
        collision.transform.SetParent(null);
    }
}
'''
