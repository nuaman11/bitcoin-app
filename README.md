# bitcoin-app
bitcoin app/to see the current value





import UIKit
import Alamofire
import SwiftyJSON

class ViewController: UIViewController, UIPickerViewDelegate, UIPickerViewDataSource{
   
    
    @IBOutlet weak var bitcoinPriceLabel: UILabel!
    
    @IBOutlet weak var pickerviewLabel: UIPickerView!
    
    @IBOutlet weak var highvalue: UILabel!
    
    @IBOutlet weak var lowvalue: UILabel!
    
    let baseURL = "https://apiv2.bitcoinaverage.com/indices/global/ticker/BTC"
      
       let currencyArray = ["AUD", "BRL","CAD","CNY","EUR","GBP","HKD","IDR","ILS","INR","JPY","MXN","NOK","NZD","PLN","RON","RUB","SEK","SGD","USD","ZAR"]
       
    var finalURl = " "
       var currencySymbolArray = ["$", "R$", "$", "Â¥", "â‚¬", "Â£", "$", "Rp", "â‚ª", "â‚¹", "Â¥", "$", "kr", "$", "zÅ‚", "lei", "â‚½", "kr", "$", "$", "R"]
    var currencySelected = " "

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }
    func getBitcoinData(url: String){
        
        Alamofire.request(url, method: .get).responseJSON { response in if response.result.isSuccess {
              

                  print("success got the bitcoin data")
                  let bitcoinJSON : JSON = JSON(response.result.value!)
                  self.updateBitcoinData(json: bitcoinJSON)
              }
                  else {
                  print("Error: \(String(describing: response.result.error))")
                  self.bitcoinPriceLabel.text = "connection issues"

            }
                
            }
    }
//JSON PARSING
    func updateBitcoinData(json : JSON){
        if let bitcoinResult = json["ask"].double {
            
            bitcoinPriceLabel.text = "\(currencySelected)\(bitcoinResult)"
        } else {
            bitcoinPriceLabel.text = "Price unavailable"
        }
        
        /***HIGH VALUE**********************************************************/
        if let bitcoinResult = json["high"].double {
            
            highvalue.text = "\(currencySelected)\(bitcoinResult)"
        } else {
            highvalue.text = "Price unavailable"
        }
        
        //**LOW VALUE**********************************
        if let bitcoinResult = json["low"].double {
            
            lowvalue.text = "\(currencySelected)\(bitcoinResult)"
        } else {
            lowvalue.text = "Price unavailable"
        }
        
        
}
    func numberOfComponents(in pickerView: UIPickerView) -> Int {
        return 1
    }
    
    func pickerView(_ pickerView: UIPickerView, numberOfRowsInComponent component: Int) -> Int {
        return currencyArray.count
    }
    
    func pickerView(_ pickerView: UIPickerView, titleForRow row: Int, forComponent component: Int) -> String? {
        return currencyArray[row]
    }
    func pickerView(_ pickerView: UIPickerView, didSelectRow row: Int, inComponent component: Int) {
        finalURl = baseURL + currencyArray[row]
        print(finalURl)
        getBitcoinData(url: finalURl)
    }

}
