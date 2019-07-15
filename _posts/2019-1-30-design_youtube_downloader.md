# title: design youtube downloader

## spec
1. request를 날릴 때 마다, youtube download가 돌아가야한다.
2. worker의 로드 밸런싱이 잘 되어야 한다.
3. worker의 경우 항상 50%가 사용되어야 한다.
	* threshold를 다음과 같이 잡는다:
		* 기준: worker 3개 # dynamic 하게 바꿔야 한다.
		* 20% working: worker가 기준 미만이면 30분마다 하나씩 떨어짐. # 30분도 allocation 잘하면 좋다.
		* 50% 이상: worker를 지금 전체 들어온 work 수의 2배가 될 만큼 hire한다.
4. 

### balance server spec
*기능(param): response 형태로 명시*  <br>
* user:
	* post youtube download: request id
	* get status(request id): completion_percentage, s3_url?
	* isYoutubedlDead

* worker:
	* probably zookeper later
		* push_worker_id(worker_id)
		* delete_worker(worker_id) # pop_worker도 있어야겠지.
		* post_work_to_worker
		* worker_heartbeat_update
		* get_work_done
	
	* for errors
		* isYoutubedlDead: yes or no

	* rest
		* create_worker(S3_path, receipt_db_path): worker_path(list) # 그냥 간단하게 spot instance당 2개씩 docker worker 띄우는 걸로
		* get_worker_working_percentage(work_id)
		* count_how_many_workers_to_hire
		* count_how_many_workers_to_fire
	 

	* timer_in_worker
		* should_worker_fired(time): worker_id?

### worker spec
*기능(param): response 형태로 명시* 
* *해당 부분은 다른 서버다*
* download yotube video


## Qs
* 실제로 퍼센티지 변화를 db에 update로 계속 때려도 되는건가?


## ports
### test db
* psql aws
	* db engine ver: psql 10.4-R1
	* DB식별자: youtubedownloaderdev
	* 마스터 사용자: voithru
	* 마스터 암호: mangosix99
	* db 이름: youtubedev
	* port: 5432

### test s3
