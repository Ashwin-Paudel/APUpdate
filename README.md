# APUpdate

Uses this code to update your Cocoa application

```Swift
import Cocoa

class UNUpdaterViewController: NSViewController {

    @IBOutlet var featureListTextView: NSTextView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do view setup here.
        do {
            let contents = try String(contentsOf: URL(string: "https://raw.githubusercontent.com/Ashwin-Paudel/download/main/newupdateinfo.txt")!)
            featureListTextView.string = contents ?? "Error Loading Features"
        } catch {
        }
//        featureListTextView.string = "hihihi"
    }
    @IBAction func updateButtonPress(_ sender: Any) {
        let applicationFolder = FileManager.default.urls(for: .applicationDirectory, in: .userDomainMask).first
        // MARK: - Stage 1: Delete the app
        do {
            try FileManager.default.removeItem(atPath: "/Applications/Unique.app")
        } catch {
            print("Error")
        }
        // MARK: - Stage 2: Download the new app
        let url = URL(string: "https://github.com/Ashwin-Paudel/download/raw/main/Unique.zip")
        FileDownloader.loadFileAsync(url: url!) { [self] (path, error) in
            let documentsUrl =  FileManager.default.urls(for: .documentDirectory, in: .userDomainMask).first!.appendingPathComponent("Unique.zip")
// MARK: - Unzip the app and save to application folder
            print("PDF File downloaded to : \(path!)")
            unzipFile(at: "/Users/ashwin/Documents/Unique.zip", to: "/Applications")
        }
    }
    func unzipFile(at sourcePath: String, to destinationPath: String) -> Bool {
        let process = Process.launchedProcess(launchPath: "/usr/bin/unzip", arguments: ["-o", sourcePath, "-d", destinationPath])
        process.waitUntilExit()
        return process.terminationStatus <= 1
    }
}
```

