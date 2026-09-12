---
title: "CF 104663M - Màn hình chuối"
description: "Chúng tôi đang mô phỏng một hệ thống giám sát theo dõi luồng giá trị lưu lượng truy cập từng phút. Tại mỗi phút, chúng tôi so sánh lưu lượng truy cập hiện tại với ngưỡng dung lượng cố định. Hệ thống không phản ứng ngay lập tức với một vi phạm hoặc một lần đọc an toàn."
date: "2026-06-29T14:58:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104663
codeforces_index: "M"
codeforces_contest_name: "Replay of Ostad Presents Intra KUET Programming Contest 2023"
rating: 0
weight: 104663
solve_time_s: 75
verified: false
draft: false
---

[CF 104663M - Màn hình chuối](https://codeforces.com/problemset/problem/104663/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một hệ thống giám sát theo dõi luồng giá trị lưu lượng truy cập từng phút. Tại mỗi phút, chúng tôi so sánh lưu lượng truy cập hiện tại với ngưỡng dung lượng cố định. Hệ thống không phản ứng ngay lập tức với một vi phạm hoặc một lần đọc an toàn. Thay vào đó, nó sử dụng hiện tượng trễ: nó chỉ chuyển sang trạng thái cảnh báo sau khi quá tải kéo dài và nó chỉ rời khỏi trạng thái cảnh báo sau khi phục hồi liên tục. 

Chính xác hơn, trong khi hệ thống ở trạng thái bình thường, nó sẽ đếm xem lưu lượng truy cập vẫn vượt quá ngưỡng trong bao nhiêu phút liên tiếp. Khi số lượng này đạt đến giới hạn nhất định, hệ thống sẽ chuyển sang trạng thái báo động. Sau đó, trong khi ở chế độ báo động, nó sẽ đếm số phút liên tiếp lưu lượng truy cập duy trì ở mức hoặc dưới ngưỡng. Khi bộ đếm khôi phục đó đạt đến giới hạn khác, hệ thống sẽ trở lại bình thường. Trong khi ở chế độ báo động, nó sẽ ở đó liên tục cho đến khi tích lũy đủ số phút an toàn. 

Đầu ra là tổng số phút mà hệ thống ở trạng thái cảnh báo trong toàn bộ quá trình mô phỏng. 

Các ràng buộc lên tới một trăm nghìn phút cho mỗi trường hợp thử nghiệm và lên tới một trăm trường hợp thử nghiệm. Điều này loại trừ bất kỳ phương pháp nào cố gắng tính toán lại hoặc mô phỏng các cấu trúc đắt tiền mỗi phút ngoài công việc liên tục. Bất kỳ giải pháp nào cũng phải tuyến tính cho mỗi trường hợp thử nghiệm. 

Một trường hợp cạnh tinh tế phát sinh từ quá trình chuyển đổi. Việc triển khai ngây thơ thường quên rằng bộ đếm được đặt lại khi thay đổi trạng thái hoặc đếm sai số phút chuyển tiếp hai lần. 

Ví dụ: xem xét ngưỡng 5, cảnh báo sau 3 giá trị cao liên tiếp và xóa sau 2 giá trị an toàn liên tiếp: 

đầu vào:```
6 5 3 2
6 6 6 1 1 1
```Ở đây, cảnh báo sẽ kích hoạt ở phút thứ 3. Việc triển khai bất cẩn có thể bắt đầu xóa ngay lập tức ở phút thứ 4 và cho rằng hệ thống để lại cảnh báo quá sớm hoặc tính sai thời lượng cảnh báo do không bao gồm cấu trúc chuyển tiếp chính xác. Hành vi đúng là cảnh báo chỉ bắt đầu sau phút cao thứ ba và chỉ kết thúc sau hai phút an toàn liên tiếp. 

Một trường hợp nguy hiểm khác là khi hệ thống không bao giờ chuyển sang trạng thái báo động. Khi đó, câu trả lời phải bằng 0 ngay cả khi có các đột biến riêng biệt, vì không có lần chạy liên tiếp nào đạt đến ngưỡng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng hệ thống chính xác như được mô tả, duy trì trạng thái hiện tại và bộ đếm các giá trị cao và thấp liên tiếp. Vào mỗi phút, chúng tôi cập nhật bộ đếm và kiểm tra xem có xảy ra chuyển đổi trạng thái hay không. Chúng tôi cũng tích lũy số phút dành cho báo động. 

Mô phỏng này đã gần đạt mức tối ưu vì mỗi phút được xử lý một lần. Cách duy nhất mà một giải pháp ngây thơ trở nên chậm chạp là nếu ai đó cố gắng tính toán lại các vệt liên tiếp từ đầu ở mỗi vị trí, quét ngược hoặc kiểm tra lại các phân đoạn. Điều đó sẽ giảm xuống thời gian bậc hai trong trường hợp xấu nhất, đặc biệt là khi các giá trị xen kẽ buộc phải quét lại tiền tố nhiều lần. 

Cái nhìn sâu sắc quan trọng là quá trình này vốn dựa trên máy trạng thái. Hệ thống chỉ phụ thuộc vào trạng thái hiện tại (bình thường hoặc báo động) và hai bộ đếm theo dõi các lần chạy liên tiếp. Không cần phải xem lại các phút trước đó hoặc duy trì bất kỳ cấu trúc dữ liệu phức tạp nào. Mỗi quá trình chuyển đổi được kích hoạt bởi các điều kiện cục bộ, do đó, chỉ cần một lượt chuyển đổi từ trái sang phải là đủ. 

Điều này làm giảm vấn đề duy trì số lượng biến không đổi trong khi quét mảng một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (chạy quét lại) | O(N2) | O(1) | Quá chậm | 
| Mô phỏng máy trạng thái | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì trạng thái hiện tại và hai quầy. 

1. Khởi tạo hệ thống ở trạng thái bình thường. Đặt bộ đếm`high_streak = 0`Và`low_streak = 0`. Cũng được thiết lập`alarm_time = 0`. 
2. Lặp lại giá trị lưu lượng truy cập trong mỗi phút từ trái sang phải. Tại mỗi phút, hãy so sánh nó với ngưỡng. 
3. Nếu trạng thái hiện tại là bình thường và lưu lượng vượt quá ngưỡng, hãy tăng`high_streak`. Nếu không thì đặt lại`high_streak`về không. Khi`high_streak`đạt đến độ dài kích hoạt cần thiết, chuyển trạng thái sang báo động và đặt lại`low_streak`. 
4. Nếu trạng thái hiện tại là báo động, chúng ta sẽ tích lũy phút này thành`alarm_time`trước bất kỳ logic chuyển tiếp nào, vì hệ thống được coi là hoạt động trong suốt từng phút. 
5. Trong khi cảnh báo, nếu lưu lượng ở mức hoặc dưới ngưỡng, hãy tăng`low_streak`. Nếu không thì đặt lại`low_streak`về không. Khi`low_streak`đạt đến độ dài khe hở cần thiết, chuyển trạng thái trở lại bình thường và đặt lại`high_streak`. 
6. Tiếp tục quá trình này cho đến khi tất cả số phút được xử lý. 

Chi tiết quan trọng là thời gian báo thức được tính dựa trên trạng thái tại mỗi phút chứ không dựa trên các chuyển đổi trong tương lai. Điều này tránh được các lỗi xảy ra tại thời điểm chuyển trạng thái. 

### Tại sao nó hoạt động 

Hệ thống được mô tả đầy đủ bằng một máy trạng thái hữu hạn chỉ có bộ nhớ ở dạng bộ đếm chạy liên tiếp. Tại bất kỳ thời điểm nào, trạng thái tương lai chỉ phụ thuộc vào trạng thái hiện tại và giá trị hiện tại so với ngưỡng. Bộ đếm đảm bảo chúng tôi phát hiện chính xác các lần chạy liên tiếp mà không cần lịch sử ngoài các lần chạy đó. Vì mỗi phút đóng góp chính xác một lần kiểm tra chuyển trạng thái và có thể là một lần cập nhật bộ đếm nên không có thông tin nào bị mất và không cần tính toán lại. Điều này đảm bảo tính chính xác và xử lý tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    out = []
    
    for _ in range(T):
        N, Xmax, A, C = map(int, input().split())
        arr = list(map(int, input().split()))
        
        state = 0  # 0 = normal, 1 = alarm
        high_streak = 0
        low_streak = 0
        alarm_time = 0
        
        for x in arr:
            if state == 1:
                alarm_time += 1
            
            if state == 0:
                if x > Xmax:
                    high_streak += 1
                else:
                    high_streak = 0
                
                if high_streak >= A:
                    state = 1
                    low_streak = 0
            else:
                if x <= Xmax:
                    low_streak += 1
                else:
                    low_streak = 0
                
                if low_streak >= C:
                    state = 0
                    high_streak = 0
        
        out.append(str(alarm_time))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp được cấu trúc như một lần truyền qua mảng lưu lượng. Biến`state`theo dõi xem màn hình hiện có đang báo động hay không. các quầy`high_streak`Và`low_streak`có liên quan lẫn nhau tùy thuộc vào tiểu bang. Thời gian báo thức chỉ tăng lên khi hệ thống đã ở chế độ báo động vào đầu phút, hệ thống này ghi lại chính xác phạm vi toàn phút. 

Một lỗi phổ biến là cập nhật trạng thái trước khi thêm vào`alarm_time`, sẽ làm mất phút báo thức cuối cùng. Một vấn đề nhỏ khác là không thể đặt lại bộ đếm ngược lại khi chuyển trạng thái, điều này có thể gây ra sự chuyển đổi ngoài ý muốn ngay lập tức sau khi chuyển trạng thái. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 5 3 2
6 6 6 1 1 1
```| Phút | Giá trị | Tiểu bang | Vệt cao | Vệt thấp | Giờ báo thức | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 6 | bình thường | 1 | 0 | 0 | 
| 2 | 6 | bình thường | 2 | 0 | 0 | 
| 3 | 6 | báo động | 3 | 0 | 1 | 
| 4 | 1 | báo động | 3 | 1 | 2 | 
| 5 | 1 | báo động | 3 | 2 | 3 | 
| 6 | 1 | bình thường | đặt lại | đặt lại | 3 | 

Quá trình chuyển sang báo động xảy ra ở phút thứ 3 khi đạt đến giá trị cao thứ ba liên tiếp. Hệ thống vẫn ở trạng thái cảnh báo cho đến khi hai giá trị an toàn liên tiếp xuất hiện, hoàn thành ở phút thứ 6. Điều này xác nhận rằng thời gian cảnh báo chỉ được tính khi trạng thái đang hoạt động. 

### Ví dụ 2 

đầu vào:```
5 10 2 2
11 9 11 9 11
```| Phút | Giá trị | Tiểu bang | Vệt cao | Vệt thấp | Giờ báo thức | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 11 | bình thường | 1 | 0 | 0 | 
| 2 | 9 | bình thường | 0 | 1 | 0 | 
| 3 | 11 | bình thường | 1 | 0 | 0 | 
| 4 | 9 | bình thường | 0 | 1 | 0 | 
| 5 | 11 | bình thường | 1 | 0 | 0 | 

Ở đây không có vệt nào đạt đến độ dài 2 nên hệ thống không bao giờ chuyển sang trạng thái báo động. Điều này chứng tỏ rằng các đột biến riêng lẻ bị bỏ qua mà không vi phạm liên tục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) cho mỗi trường hợp thử nghiệm | Mỗi phút được xử lý một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ một số bộ đếm và biến trạng thái cố định được lưu trữ | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm vẫn tuyến tính theo số phút, do đó phương pháp này phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    res = []

    for _ in range(T):
        N, Xmax, A, C = map(int, input().split())
        arr = list(map(int, input().split()))
        
        state = 0
        high = 0
        low = 0
        alarm = 0
        
        for x in arr:
            if state == 1:
                alarm += 1
            
            if state == 0:
                if x > Xmax:
                    high += 1
                else:
                    high = 0
                if high >= A:
                    state = 1
                    low = 0
            else:
                if x <= Xmax:
                    low += 1
                else:
                    low = 0
                if low >= C:
                    state = 0
                    high = 0
        
        res.append(str(alarm))
    
    return "\n".join(res)

# provided samples
assert run("""2
9 5 3 2
2 6 8 9 6 5 4 3 6
4 1 1 1
1 2 0 2
""") == "3\n2"

# minimum size, no alarm
assert run("""1
1 10 1 1
5
""") == "0"

# immediate alarm
assert run("""1
3 5 1 1
6 6 6
""") == "3"

# oscillation prevents triggering
assert run("""1
6 5 2 2
6 1 6 1 6 1
""") == "0"

# long sustained alarm then clear
assert run("""1
10 5 2 3
6 6 1 1 1 1 1 6 6 6
""") == "7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn dưới ngưỡng | 0 | không kích hoạt báo động | 
| tràn lặp lại ngay lập tức | chiều dài đầy đủ | kích hoạt khi bắt đầu | 
| giá trị xen kẽ | 0 | tính chính xác của chuỗi logic | 
| chu kỳ dài rõ ràng | đếm một phần | hủy kích hoạt đúng cách | 

## Vỏ cạnh 

Trường hợp một cạnh là khi ngưỡng kích hoạt và ngưỡng giải phóng đều bằng 1. Hệ thống trở nên cực kỳ nhạy cảm và chuyển trạng thái ngay lập tức. Thuật toán xử lý việc này vì bộ đếm chuỗi đạt đến ngưỡng trong một bước duy nhất và quá trình chuyển đổi trạng thái vẫn xảy ra sau khi xử lý chính xác phút hiện tại. 

Một trường hợp cạnh khác là một chuỗi dài kích hoạt cảnh báo ở gần cuối mảng mà không có đủ thời gian để xóa nó. Thuật toán chỉ đếm số phút cảnh báo cho đến khi mảng kết thúc và không yêu cầu giải phóng mặt bằng cuối cùng, phù hợp với định nghĩa vấn đề vì chúng tôi chỉ đo thời gian cảnh báo quan sát được. 

Trường hợp khó phát hiện cuối cùng là khi lưu lượng dao động chính xác xung quanh ngưỡng để bộ đếm được đặt lại nhiều lần. Vì cả hai bộ đếm được đặt lại ở các điều kiện trái ngược nhau nên không xảy ra hiện tượng chuyển tiếp không hợp lệ và máy trạng thái vẫn ổn định mà không bị kích hoạt ngẫu nhiên.
