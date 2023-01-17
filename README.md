ChatGPT Application with flutter
ChatGPT is a chatbot launched by OpenAI in November 2022. It is built on top of OpenAI's GPT-3.5 family of large language models, and is fine-tuned with both supervised and reinforcement learning techniques.

Install Package
chat_gpt:1.0.1+5
pub get
Example
Create ChatGPT Instance

Parameter
Token
Your secret API keys are listed below. Please note that we do not display your secret API keys again after you generate them.
Do not share your API key with others, or expose it in the browser or other client-side code. In order to protect the security of your account, OpenAI may also automatically rotate any API key that we've found has leaked publicly.
https://beta.openai.com/account/api-keys
OrgId
Identifier for this organization sometimes used in API requests
https://beta.openai.com/account/org-settings
final openAI = ChatGPT.instance.builder("token", baseOption: HttpSetup(receiveTimeout: 6000));
Text Complete API
Translate Method
translateEngToThai
translateThaiToEng
translateToJapanese
Model
kTranslateModelV3
kTranslateModelV2
kCodeTranslateModelV2
Translate natural language to SQL queries.
Create code to call the Stripe API using natural language.
Find the time complexity of a function.
https://beta.openai.com/examples
final request = CompleteReq(prompt: translateEngToThai(word: ''),
model: kTranslateModelV3, max_tokens: 200);

openAI.onCompleteStream(request:request).listen((response) => print(response));
Example Q&A
Answer questions based on existing knowledge.
final request = CompleteReq(prompt:'What is human life expectancy in the United States?'),
model: kTranslateModelV3, max_tokens: 200);

openAI.onCompleteStream(request:request).listen((response) => print(response));
Request
Q: What is human life expectancy in the United States?
Response
A: Human life expectancy in the United States is 78 years.
Model List
List and describe the various models available in the API. You can refer to the Models documentation to understand what models are available and the differences between them.
https://beta.openai.com/docs/api-reference/models
final models = await openAI.listModel();
Engine List
Lists the currently available (non-finetuned) models, and provides basic information about each one such as the owner and availability.
https://beta.openai.com/docs/api-reference/engines
final engines = await openAI.listEngine();
Generation Image
prompt
A text description of the desired image(s). The maximum length is 1000 characters.
n
The number of images to generate. Must be between 1 and 10.
size
The size of the generated images. Must be one of 256x256, 512x512, or 1024x1024.
response_format
The format in which the generated images are returned. Must be one of url or b64_json.
user
A unique identifier representing your end-user, which can help OpenAI to monitor and detect abuse.
openAI = ChatGPT
.instance
.builder("token",
baseOption: HttpSetup(receiveTimeout: 7000));

const prompt = "Snake Red";

final request = GenerateImage(prompt,2);

               openAI.generateImageStream(request)
              .asBroadcastStream()
              .listen((it) {
                print(it.data?.last?.url)
              });

/// cancel stream controller
openAI.genImgClose();
Flutter Example
class _TranslateScreenState extends State<TranslateScreen> {
/// text controller
final _txtWord = TextEditingController();

CompleteRes? _response;
StreamSubscription? subscription;

final api = ChatGPT.instance;

void _translateEngToThai() {
final request = CompleteReq(
prompt: translateEngToThai(word: _txtWord.text.toString()),
model: kTranslateModelV3,
max_tokens: 1000);
subscription = ChatGPT.instance
.builder("token")
.onCompleteStream(request: request)
.listen((res) {
setState(() {
_response = res;
});
});
}

void modelDataList() async{
final model = await ChatGPT.instance
.builder("token")
.listModel();

}

void engineList() async{
final engines = await ChatGPT.instance
.builder("token")
.listEngine();
}

@override
void dispose() {
subscription?.cancel();
super.dispose();
}

@override
Widget build(BuildContext context) {
var size = MediaQuery.of(context).size;
return Scaffold(
backgroundColor: Colors.white,
body: SingleChildScrollView(
child: Center(
child: Padding(
padding: const EdgeInsets.symmetric(vertical: 16.0),
child: Column(
crossAxisAlignment: CrossAxisAlignment.center,
children: [
/**
* title translate
*/
_titleCard(size),
/**
* input card
* insert your text for translate to th.com
*/
_inputCard(size),

                /**
                 * card input translate
                 */
                _resultCard(size),
                /**
                 * button translate
                 */
                _btnTranslate()
              ],
            ),
          ),
        ),
      ),
      bottomNavigationBar: _navigation(size),
    );
}
}