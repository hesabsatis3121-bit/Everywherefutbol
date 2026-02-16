using UnityEngine;

public class PlayerController : MonoBehaviour
{
    private CharacterController controller;
    private Vector3 direction;
    public float forwardSpeed = 10f;

    private int desiredLane = 1; // 0:Sol, 1:Mərkəz, 2:Sağ
    public float laneDistance = 4f; // Zolaqlar arası məsafə

    public float jumpForce = 10f;
    public float Gravity = -20f;

    void Start()
    {
        controller = GetComponent<CharacterController>();
    }

    void Update()
    {
        direction.z = forwardSpeed;

        // Tullanma məntiqi
        if (controller.isGrounded)
        {
            if (Input.GetKeyDown(KeyCode.UpArrow))
            {
                Jump();
            }
        }
        else
        {
            direction.y += Gravity * Time.deltaTime;
        }

        // Zolaq dəyişdirmə (Sağ-Sol)
        if (Input.GetKeyDown(KeyCode.RightArrow))
        {
            desiredLane++;
            if (desiredLane == 3) desiredLane = 2;
        }
        if (Input.GetKeyDown(KeyCode.LeftArrow))
        {
            desiredLane--;
            if (desiredLane == -1) desiredLane = 0;
        }

        // Gələcək mövqeyi hesabla
        Vector3 targetPosition = transform.position.z * transform.forward + transform.position.y * transform.up;

        if (desiredLane == 0)
            targetPosition += Vector3.left * laneDistance;
        else if (desiredLane == 2)
            targetPosition += Vector3.right * laneDistance;

        // Hərəkəti tətbiq et
        Vector3 moveDelta = targetPosition - transform.position;
        controller.Move(moveDelta + direction * Time.deltaTime);
    }

    private void Jump()
    {
        direction.y = jumpForce;
    }
}
