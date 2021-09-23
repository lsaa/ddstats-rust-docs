# Rust Code Funnies

### Cool Error Stuff
```
// If you can't / don't want to use Anyhow
type Result<T> = std::result::Result<T, Box<dyn std::error::Error + Send + Sync>>;
```

### Tokio interval loop
```
let mut interval = tokio::time::interval(Duration::from_secs_f32(1. / 3.));
loop {
	interval.tick().await;
	// Do Stuff
}
```

### Tokio block on synchronous context
```
let r = futures::executor::block_on({future});
```

### Serde serialize / deserialize json
```
// serde_json = "1.0"

// Serialize where obj impl serde::Serialize
let serialized = serde_json::to_string(&obj).unwrap();

// Deserialize from str where Obj impl serde::Deserialize
let deserialized: Obj = serde_json::from_str(json).unwrap();
```

### Hyper Request with HTTPS and timeout
```
pub async fn get_ddstats_memory_marker(os: OperatingSystem) -> Result<MarkerResponse> {
    let https = HttpsConnector::new();
    let client = Client::builder().build::<_, hyper::Body>(https);
    let path = format!("api/process-memory/marker?operatingSystem={:?}", os);
    let uri = format!("https://devildaggers.info/{}", path);
    let req = Request::builder()
        .header("accept", "application/json")
        .method(Method::GET)
        .uri(uri)
        .body(Body::empty())
        .unwrap();
    log::info!("Attempting to pull marker");
    let mut res = tokio::time::timeout(Duration::from_secs(2), client.request(req)).await??;
    log::info!("Pulled Marker Sucessfully");
    let mut body = Vec::new();
    while let Some(chunk) = res.body_mut().next().await {
        body.extend_from_slice(&chunk?);
    }
    let res: MarkerResponse = serde_json::from_slice(&body)?;
    Ok(res)
}
```

### Log Stuff
Logs to file and panics.
```
// log = "0.4.14"
// simple-logging = "2.0.2"
// log-panics = "2.0.0"

// Setup Logs
log_to_file("debug_logs.txt", log::LevelFilter::Info).expect("Couldn't create logger!");
log_panics::init();
```

### Minimize release filesize
```
cargo-features = ["strip"]

[profile.release]
strip = true
lto = true
codegen-units = 1
```

### Enum to and from String
```
// To string (must impl Debug)
let s = format!("{:?}", my_value);

// FromStr
impl FromStr for ColorProxy {
    type Err = ();

    fn from_str(input: &str) -> Result<ColorProxy, Self::Err> {
        match input {
            "Reset" => Ok(ColorProxy(Color::Reset)),
            "Black" => Ok(ColorProxy(Color::Black)),
            "Red" => Ok(ColorProxy(Color::Red)),
            "Green" => Ok(ColorProxy(Color::Green)),
            "Yellow" => Ok(ColorProxy(Color::Yellow)),
            "Blue" => Ok(ColorProxy(Color::Blue)),
            "Magenta" => Ok(ColorProxy(Color::Magenta)),
            "Cyan" => Ok(ColorProxy(Color::Cyan)),
            "Gray" => Ok(ColorProxy(Color::Gray)),
            "DarkGray" => Ok(ColorProxy(Color::DarkGray)),
            "LightRed" => Ok(ColorProxy(Color::LightRed)),
            "LightGreen" => Ok(ColorProxy(Color::LightGreen)),
            "LightYellow" => Ok(ColorProxy(Color::LightYellow)),
            "LightBlue" => Ok(ColorProxy(Color::LightBlue)),
            "LightMagenta" => Ok(ColorProxy(Color::LightMagenta)),
            "LightCyan" => Ok(ColorProxy(Color::LightCyan)),
            "White" | _ => Ok(ColorProxy(Color::White)),
        }
    }
}

// With strum

// strum = "0.21"
// strum_macros = "0.21"

extern crate strum;
#[macro_use] extern crate strum_macros;

#[derive(EnumString)]
enum Color {
    Red,
    Green,
    Blue,
}
```

### Ron Deserialization/Serialization
```
// Serialization
let pretty = PrettyConfig::new();
let s = to_string_pretty(&r, pretty).expect("Serialization failed");

// Deserialization
use ron::de::from_reader; // From Buffers (Files, etc..)
use ron::de::from_str;

let f = File::open(&get_priority_file()).expect("Failed opening file");

let read_from_str: Obj = from_str("{valid ron}").expect("Coudln't deserialize");
let read_from_reader: Obj = from_reader(f).expect("Coudln't deserialize");
```

### Regex
```
// regex = "1.5.4"

let re = Regex::new(r"clr-set\s(\w*)\s(\w*)\s(\d*)\s(\d*)\s(\d*)\s(\w*)\s(\d*)\s(\d*)\s(\d*)");
let re = re.unwrap();

for cap in re.captures_iter({string}) {
	let (a, b) = (cap.get(1).unwrap(), cap.get(2).unwrap());
}
```