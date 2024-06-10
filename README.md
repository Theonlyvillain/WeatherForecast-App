# WeatherForecast-App
Sihle Phasoane 
ST10348953

Purpose of App :
The purpose of the app is to provide users with a convenient and accessible way to access weather forecasts for the upcoming week. Users can view detailed information about the weather conditions for each day, including temperature, precipitation, and other relevant data. The app also calculates and displays the average temperature for the entire week, allowing users to plan their activities accordingly. With intuitive navigation and interactive features, users can easily switch between screens to find the information they need, making it a useful tool for staying informed about the weather.

Source Code :
import android.content.Intent
import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.*
import androidx.appcompat.app.AppCompatActivity
import com.example.weatherapp.R

class MainActivity : AppCompatActivity() {

    private var listView: ListView? = null
    private var btnDetailedView: Button? = null
    private var tvAverageTemperature: TextView? = null
    private var weatherDataArrayList: ArrayList<WeatherData> = ArrayList()

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        listView = findViewById(R.id.listView)
        btnDetailedView = findViewById(R.id.btnDetailedView)
        tvAverageTemperature = findViewById(R.id.tvAverageTemperature)

        weatherDataArrayList.add(WeatherData("Monday", 20))
        weatherDataArrayList.add(WeatherData("Tuesday", 22))
        weatherDataArrayList.add(WeatherData("Wednesday", 25))
        weatherDataArrayList.add(WeatherData("Thursday", 18))
        weatherDataArrayList.add(WeatherData("Friday", 21))
        weatherDataArrayList.add(WeatherData("Saturday", 23))
        weatherDataArrayList.add(WeatherData("Sunday", 24))

        val weatherAdapter = WeatherAdapter(this, weatherDataArrayList)
        listView?.adapter = weatherAdapter

        btnDetailedView?.setOnClickListener {
            val intent = Intent(this, DetailedViewActivity::class.java)
            startActivity(intent)
        }

        var totalTemperature = 0
        for (data in weatherDataArrayList) {
            totalTemperature += data.temperature
        }
        val averageTemperature = totalTemperature.toDouble() / weatherDataArrayList.size
        tvAverageTemperature?.text = "Average Temperature for the Week: $averageTemperature°C"
    }
}

data class WeatherData(val day: String, val temperature: Int)

class WeatherAdapter(
    context: AppCompatActivity,
    private val weatherDataArrayList: ArrayList<WeatherData>
) : ArrayAdapter<WeatherData>(context, 0, weatherDataArrayList) {
    override fun getView(position: Int, convertView: View?, parent: ViewGroup): View {
        var listItem = convertView
        if (listItem == null) {
            listItem = LayoutInflater.from(context).inflate(R.layout.activity_main, parent, false)
        }

        val currentWeather = weatherDataArrayList[position]

        val dayTextView: TextView = listItem!!.findViewById(android.R.id.text1)
        dayTextView.text = currentWeather.day

        val temperatureTextView: TextView = listItem.findViewById(android.R.id.text2)
        temperatureTextView.text = currentWeather.temperature.toString()

        return listItem
    }
}

class DetailedViewActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_detailed_view)
    }
}
