package sspcoolgay.project.com.mylinearlayoutex;

        import android.app.Activity;
        import android.app.AlertDialog;
        import android.content.DialogInterface;
        import android.os.Bundle;
        import android.util.SparseBooleanArray;
        import android.view.View;
        import android.view.Window;
        import android.view.inputmethod.InputMethodManager;
        import android.widget.ArrayAdapter;
        import android.widget.Button;
        import android.widget.EditText;
        import android.widget.ListView;
        import android.widget.TextView;
        import android.widget.Toast;
        import java.util.ArrayList;

public class MyLinearLayoutEx extends Activity implements View.OnClickListener {
    private Button addButton;
    private Button deleteButton;
    private Button resultButton;
    private EditText editText1;
    private TextView textView1;
    private ListView listView1;

    ArrayList<String> foodList = new ArrayList<String>();
    ArrayAdapter<String> adapter;
    String[] array = foodList.toArray(new String[foodList.size()]);
    String _editText1;



    @Override
    public void onCreate(Bundle icicle) {
        super.onCreate(icicle);
        requestWindowFeature(Window.FEATURE_NO_TITLE);

        setContentView(R.layout.main);

        addButton       = (Button) findViewById(R.id.addButton);
        deleteButton    = (Button) findViewById(R.id.deleteButton);
        resultButton    = (Button) findViewById(R.id.resultButton);
        editText1       = (EditText)findViewById(R.id.etxt1);
        listView1       = (ListView)findViewById(R.id.lstvw1);
        textView1       = (TextView)findViewById(R.id.txtvw1);

        addButton.setOnClickListener(this);
        resultButton.setOnClickListener(this);
        deleteButton.setOnClickListener(this);

        adapter = new ArrayAdapter<String>(this,R.layout.listview_custom_layout,foodList);
        listView1.setChoiceMode(ListView.CHOICE_MODE_MULTIPLE);
        listView1.setAdapter(adapter);
    }

    @Override
    public void onClick(View view) {
        if (view == addButton) {
            _editText1 = editText1.getText().toString();
            if(_editText1.length()==0) showDialog(this,"","음식 후보를 적어주세요");
            else {
                foodList.add(_editText1);
                editText1.setText(null);
                listView1.setSelection(adapter.getCount() - 1);
                Toast.makeText(getApplicationContext(), "등록 :" + foodList, Toast.LENGTH_SHORT).show();
            }
        }

        if (view == deleteButton) {
            SparseBooleanArray sba = listView1.getCheckedItemPositions();
            if(sba.size() != 0){
                for(int i = listView1.getCount()-1; i >=0; i-- ) {
                    if (sba.get(i)) {
                        foodList.remove(i);
                    }
                }
            }

            listView1.clearChoices();
            adapter.notifyDataSetChanged();
        }

        if (view == resultButton) {
            if(foodList.size()==0 ) showDialog(this,"","음식을 등록해 주세요.");
            else {
                textView1.setText(foodList.get((int) (Math.random() * foodList.size())));
                InputMethodManager im = (InputMethodManager) getSystemService(INPUT_METHOD_SERVICE);
                im.hideSoftInputFromWindow(getCurrentFocus().getWindowToken(), InputMethodManager.HIDE_NOT_ALWAYS);
            }
        }
    }


    private static void showDialog(final Activity activity, String title, String text) {
        AlertDialog.Builder ad=new AlertDialog.Builder(activity);
        ad.setTitle(title);
        ad.setMessage(text);
        ad.setPositiveButton("OK",new DialogInterface.OnClickListener() {
            public void onClick(DialogInterface dialog, int whichButton) {
                activity.setResult(Activity.RESULT_OK);
            }
        });
        ad.create();
        ad.show();
    }
}




