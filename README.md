# tf-serving-gpu
TF Serving GPU

source .kubectl_aliases
k apply -f gpu.yaml
k exec -it ms-gpu-test -n gpu -c tfs-container -- bash

tfs --port=8500 --rest_api_port=8501 --model_name=my_model --model_base_path=/serving/tensorflow_serving/servables/tensorflow/testdata/saved_model_half_plus_two_gpu/

curl -L https://s3-us-west-2.amazonaws.com/test-serving/inception.zip > inception.zip && unzip inception.zip
mlf_tfs_model_server --port=8500 --rest_api_port=8501 --model_name=inception --model_base_path=/inception

Load test client setup:
	curl -L wget https://github.com/tsenart/vegeta/releases/download/cli%2Fv12.1.0/vegeta-12.1.0-linux-amd64.tar.gz > vegata.tar.gz
tar xvf vegata.tar.gz
rm -f vegeta-12.5.1-linux-amd64.tar.gz
chmod u+rx vegeta
	cp vegeta /usr/local/bin
	
git clone https://github.com/sreddybr3/tf-serving-gpu
vegeta attack -rate=4 -duration=10s -timeout=100s -targets=targets.txt -insecure | vegeta report

k delete -f gpu.yaml
