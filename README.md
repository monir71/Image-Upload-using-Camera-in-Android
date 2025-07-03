See the layout:

    <ImageView
        android:layout_width="400sp"
        android:layout_height="400sp"
        android:scaleType="fitXY"
        android:background="#f00"
        android:id="@+id/imgCamera"/>

    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/imgBtn"
        android:layout_marginTop="10sp"
        android:text="Take Photo"/>

Now in MainActivity.java we set OnClickListener to the imgBtn.
Passing an Intent iCamera to startActivityForResult.
We have set CAMERA_REQ_CODE as 100. It is necessary because different
input resources use the override method onActivityResult() where
we can determine the exact action from requestCode.
We created a Bitmap image from data.getExtras().get("data") by typecasting.
Now setImageBitmap to the ImageView imgCamera.
 

public class MainActivity extends AppCompatActivity {
    private final int CAMERA_REQ_CODE = 100;
    ImageView imgCamera;
    Button imgBtn;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        EdgeToEdge.enable(this);
        setContentView(R.layout.activity_main);

        imgCamera = findViewById(R.id.imgCamera);
        imgBtn = findViewById(R.id.imgBtn);

        imgBtn.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Intent iCamera = new Intent(MediaStore.ACTION_IMAGE_CAPTURE);
                startActivityForResult(iCamera, CAMERA_REQ_CODE);
            }
        });

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main), (v, insets) -> {
            Insets systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars());
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom);
            return insets;
        });
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, @Nullable Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        if(resultCode == RESULT_OK)
        {
            if(requestCode == CAMERA_REQ_CODE)
            {
                Bitmap img = (Bitmap) data.getExtras().get("data");
                imgCamera.setImageBitmap(img);
            }
        }

    }
}
