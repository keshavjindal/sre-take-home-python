# README

## Installation and Running the Code

1. Clone the repository:
   git clone https://github.com/keshavjindal/sre-take-home-python.git

2. Go inside the directory:
   cd sre-take-home-python

3. Install dependencies:
   pip install -r requirements.txt

4. Run the monitoring script (main.py) with the configuration file (sample.yaml):
   python main.py sample.yaml

## What were the issues in main.py and how I identified them:

1. There were 4 test cases in sample.yaml, which I ran one by one by commenting out the others while testing a particular case.

2. I started with the second test case: sample index up.
When I ran the script, I got an error related to the "method" variable. I realized that when the method is not provided in the YAML, it should default to GET, as stated in the instructions PDF. However, in the original code, it remained None. So, I added a check to default it to "GET" if it's missing.

3. Next, I ran the first test case: sample body up.
The request ran, but the availability was coming out as 0%. I added debug logs and printed the API response, which showed: "Failed to deserialize the JSON body"
I realized that the JSON string from the YAML was not being converted into a proper JSON object before being sent. To fix this, I used the json package and applied json.loads() on the body before sending the request.

4. The third test case: sample body down ran as expected and correctly returned "DOWN".

5. Then I ran the last test case: sample error down.
It threw an error related to the body. Since this endpoint used the GET method and had no body, the script failed while trying to parse it. So, I added a condition to only load and pass the body if it exists.

6. After re-reading the instructions PDF, I implemented these features also:

a) To meet the availability criteria, an endpoint must respond within 500 milliseconds. I added a condition: response.elapsed.total_seconds() <= 0.5

b) The instructions mention ignoring port numbers when grouping availability by domain. I updated the domain extraction logic accordingly.

c) I also added timeout=5 to the request to prevent it from hanging indefinitely. I chose 5 seconds as a reasonable upper limit to consider a request as taking too long to respond.