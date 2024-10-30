import "./App.css";
import { useState } from "react";

function App() {
const [city, setCity] = useState("");
const [weatherDetails, setWeatherDetails] = useState({});
const [isLoading, setIsLoading] = useState(false);
const [errorMessage, setErrorMessage] = useState("");

const getData = (e) => {
e.preventDefault();
setIsLoading(true);
setErrorMessage(""); // Reset error message
fetch(
`https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=751d66e130befad396405dc13796a57c&units=metric` // Ensure 'metric' is used
)
.then((response) => {
if (!response.ok) {
throw new Error("City not found"); // Handle HTTP errors
}
return response.json();
})
.then((data) => {
setWeatherDetails(data);
setIsLoading(false);
})
.catch((error) => {
console.error(error);
setErrorMessage(error.message); // Set the error message
setIsLoading(false);
});
setCity("");
};

return (
<div className="w-[100%] h-[100vh] bg-[#4aacb1]">
<div className="max-w-[1320px] mx-auto">
<h1 className="text-[40px] font-bold py-[50px] text-white">
Simple Weather App
</h1>
<form onSubmit={getData}>
<input
type="text"
value={city}
onChange={(e) => setCity(e.target.value)}
className="w-[300px] h-[40px] pl-3"
placeholder="City Name"
/>
<button className="bg-[#4d1991] text-white px-4 py-2 rounded-lg hover:bg-[#3e0e6c] transition duration-200 ease-in-out">
Submit
</button>

        </form>

        <div className="w-[400px] mx-auto bg-white shadow-lg mt-[40px] p-[25px] relative">
          {isLoading ? (
            <img
              src="https://tenor.com/view/loading-thinking-shiba-inu-gif-25291442"
              width={100}
              className="absolute left-[40%]"
              alt="Loading"
            />
          ) : null}

          {errorMessage ? (
            <p className="text-red-500">{errorMessage}</p> // Show error message if any
          ) : Object.keys(weatherDetails).length > 0 ? (
            <>
              <h3 className="font-bold text-[30px]">
                {weatherDetails.name}{" "}
                <span className="bg-[yellow]">
                  {weatherDetails.sys.country}
                </span>
              </h3>
              <h2 className="font-bold text-[40px]">
                {weatherDetails.main.temp} °C {/* Show temperature correctly */}
              </h2>

              <img
                src={`http://openweathermap.org/img/w/${weatherDetails.weather[0].icon}.png`}
                alt={weatherDetails.weather[0].description}
              />
              <p>{weatherDetails.weather[0].main}</p>
            </>
          ) : (
            <p>No Data</p>
          )}
        </div>
      </div>
    </div>

);
}

export default App;
