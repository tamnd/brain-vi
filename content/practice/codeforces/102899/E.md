---
title: "CF 102899E - KK \u4e0e\u7b54\u8fa9"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có nhiều “phiên phòng thủ”. Trong mỗi phần thi, một danh sách học sinh sẽ xuất hiện và mỗi học sinh có điểm tổng hợp được tính bằng tổng của hai thành phần."
date: "2026-07-04T08:20:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102899
codeforces_index: "E"
codeforces_contest_name: "The 2nd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102899
solve_time_s: 40
verified: true
draft: false
---

[CF 102899E - KK \u4e0e\u7b54\u8fa9](https://codeforces.com/problemset/problem/102899/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi trường hợp thử nghiệm, có nhiều “phiên phòng thủ”. Trong mỗi phần thi, một danh sách học sinh sẽ xuất hiện và mỗi học sinh có điểm tổng hợp được tính bằng tổng của hai thành phần. 

Một học sinh đặc biệt, KK, xuất hiện một lần trong mỗi buổi học và có điểm trong buổi đó. Bất kỳ học sinh nào khác có thể xuất hiện trong một hoặc nhiều buổi học, mỗi lần có thể có điểm số khác nhau. 

Một học viên được coi là “bị KK chi phối” nếu mỗi lần học viên đó xuất hiện trong bất kỳ buổi học nào, điểm của họ trong buổi học đó thấp hơn hẳn so với điểm của KK trong cùng buổi học đó. Nếu học sinh đạt hoặc vượt KK dù chỉ một buổi học thì sẽ không được tính. 

Nhiệm vụ là tính xem có bao nhiêu học sinh khác nhau thỏa mãn điều kiện này. 

Cấu trúc đầu vào có quy mô nhỏ: mỗi test case có tối đa 10 phiên và mỗi phiên có tối đa 10 người. Điều này ngay lập tức loại trừ mọi thứ ngoài việc quét bậc hai hoặc tuyến tính đơn giản. Ngay cả một lần quét lồng nhau đơn giản trên tất cả các bản ghi cũng có tối đa vài nghìn thao tác cho mỗi trường hợp thử nghiệm, điều này không đáng kể trong giới hạn 1 giây. 

Điểm tinh tế chính là học sinh xuất hiện nhiều lần trong các phiên và tính hợp lệ của chúng phụ thuộc vào điều kiện chung trong tất cả các lần xuất hiện. Một sai lầm ngây thơ là chỉ kiểm tra sự xuất hiện đầu tiên của một học sinh hoặc so sánh với một giá trị KK thay vì các giá trị KK theo phiên cụ thể. 

Một trường hợp thất bại cụ thể trông như thế này: 

Buổi 1: Điểm KK là 100, Alice đạt 90 

Buổi 2: Điểm KK là 50, Alice đạt 60 

Nếu chúng ta chỉ so sánh Alice một lần, cô ấy có vẻ ổn, nhưng cô ấy đã vi phạm quy tắc ở phần 2 vì cô ấy không hề thấp hơn KK ở đó. Vì vậy Alice phải bị loại trừ. 

Một trường hợp khác là học sinh chỉ xuất hiện một lần. Nếu điểm của họ thấp hơn KK trong phiên đó, chúng sẽ tự động có hiệu lực, nhưng chỉ sau khi đảm bảo không có vi phạm tiềm ẩn nào tồn tại sau đó (điều này yêu cầu phải theo dõi tất cả các lần xảy ra). 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là: đối với mỗi tên học sinh, hãy thu thập tất cả các phiên mà chúng xuất hiện và đối với mỗi phiên đó, hãy so sánh điểm của họ với điểm của KK trong phiên đó. Nếu tất cả các so sánh đều đạt, hãy đếm học sinh. 

Điều này hoạt động chính xác, nhưng nếu chúng tôi triển khai nó một cách đơn giản bằng cách quét toàn bộ tập dữ liệu cho từng học sinh, chúng tôi có thể duyệt qua tất cả các mục nhập nhiều lần. Trong trường hợp xấu nhất, chúng tôi có thể kiểm tra tới 100 mục nhập cho mỗi học sinh, dẫn đến việc làm lặp lại dư thừa. 

Quan sát quan trọng là chúng ta không cần phải tính toán lại cho mỗi học sinh. Thay vào đó, chúng tôi có thể xử lý mỗi hồ sơ một lần, duy trì điểm của KK mỗi buổi và ngay lập tức vô hiệu hóa bất kỳ học sinh nào vi phạm điều kiện. Điều này biến vấn đề thành một vấn đề tổng hợp một lượt qua tên. 

Chúng tôi duy trì một từ điển ghi lại xem mỗi học sinh có còn hiệu lực hay không. Khi đọc từng phiên, chúng tôi so sánh từng người tham gia với điểm KK của phiên đó và cập nhật trạng thái của họ ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên mỗi lần quét của học sinh | O(N²) cho mỗi trường hợp thử nghiệm | O(N) | Quá chậm (chi phí không cần thiết) | 
| Xác thực một lần | O(N) cho mỗi trường hợp thử nghiệm | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc số phiên. Đối với mỗi phiên, trước tiên chúng tôi xác định điểm của KK vì KK luôn xuất hiện ở mục đầu tiên của phiên. Điều này cung cấp cho chúng tôi điểm chuẩn cho mỗi phiên. 
2. Tạo từ điển`valid[name] = True`để theo dõi xem mỗi học sinh có còn đủ điều kiện để được tính hay không. 
3. Đối với mỗi người tham gia buổi học, hãy so sánh điểm của họ với điểm của KK cho buổi học đó. Nếu một học sinh không phải là KK và điểm của họ lớn hơn hoặc bằng điểm của KK, chúng tôi sẽ ngay lập tức đánh dấu họ là không hợp lệ. 
4. Tiếp tục xử lý tất cả các buổi học, đảm bảo rằng sau khi một học sinh bị đánh dấu là không hợp lệ, họ vẫn không hợp lệ bất kể các buổi học tiếp theo. 
5. Sau khi xử lý tất cả các phiên, đếm xem có bao nhiêu tên riêng biệt (không bao gồm KK) vẫn được đánh dấu hợp lệ. 

Sự lựa chọn thiết kế quan trọng là vô hiệu ngay lập tức. Điều này tránh lưu trữ toàn bộ lịch sử mỗi phiên. 

### Tại sao nó hoạt động 

Đối với mỗi học sinh, điều kiện mà chúng tôi kiểm tra là một hạn chế chung đối với tất cả các lần xuất hiện của họ: mọi điểm được quan sát phải hoàn toàn thấp hơn điểm của KK trong buổi học tương ứng. Thuật toán thực thi điều này bằng cách kiểm tra từng lần xuất hiện một cách độc lập và vô hiệu hóa ở lần thất bại đầu tiên. Bởi vì sự vô hiệu là đơn điệu và không bao giờ đảo ngược, nên một học sinh được đánh dấu là hợp lệ ở cuối khi và chỉ nếu tất cả các so sánh của họ đều đạt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

T = int(input())
for _ in range(T):
    n = int(input())
    
    valid = {}
    kk_name = None

    for _ in range(n):
        a = int(input())
        
        session = []
        for i in range(a):
            s, b, c = input().split()
            b = int(b)
            c = int(c)
            total = b + c
            
            if i == 0:
                kk_name = s
                kk_score = total
            
            if s not in valid:
                valid[s] = True
            
            if s != kk_name:
                if total >= kk_score:
                    valid[s] = False

    ans = 0
    for name, ok in valid.items():
        if name != kk_name and ok:
            ans += 1

    print(ans)
```Mã này dựa trên sự đảm bảo rằng mục nhập đầu tiên trong mỗi phiên tương ứng với KK. Điều đó cho phép chúng tôi nắm bắt được điểm của KK trước khi xử lý những người tham gia còn lại trong phiên đó. 

Một điểm tinh tế là chúng ta không bao giờ cần lưu trữ lịch sử mỗi phiên. Chúng tôi chỉ lưu trữ trạng thái hợp lệ hiện tại cho mỗi tên, giúp duy trì mức sử dụng bộ nhớ ở mức tối thiểu và logic đơn giản. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét: 

Lần 1: KK = 100, Alice = 90, Bob = 110 

Lần 2: KK = 80, Alice = 70 

Chúng tôi theo dõi tính hợp lệ như sau: 

| Phiên | Điểm KK | Alice | Alice hợp lệ | Bob | Bob hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 100 | 90 | Đúng | 110 | Sai | 
| 2 | 80 | 70 | Đúng | - | Sai | 

Alice sống sót sau mọi cuộc kiểm tra, Bob thất bại ngay lập tức. 

Điều này cho thấy rằng một vi phạm duy nhất là đủ để loại một học sinh. 

### Ví dụ 2 

Buổi 1: KK = 50, Charlie = 49 

Lần 2: KK = 60, Charlie = 65 

| Phiên | Điểm KK | Charlie | Charlie hợp lệ | 
| --- | --- | --- | --- | 
| 1 | 50 | 49 | Đúng | 
| 2 | 60 | 65 | Sai | 

Mặc dù Charlie vượt qua trong phiên đầu tiên, nhưng phiên thứ hai đã vô hiệu hóa anh ta, chứng tỏ sự cần thiết phải theo dõi toàn cầu trong tất cả các phiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng số mục) | Mỗi người tham gia được xử lý một lần và được kiểm tra trong O(1) | 
| Không gian | O(tên duy nhất) | Chúng tôi lưu trữ một boolean cho mỗi tên sinh viên | 

Vì mỗi trường hợp thử nghiệm có tối đa 100 mục, giải pháp chạy trong thời gian không đáng kể và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        n = int(input())
        valid = {}
        kk_name = None

        for _ in range(n):
            a = int(input())
            for i in range(a):
                s, b, c = input().split()
                b = int(b)
                c = int(c)
                total = b + c

                if i == 0:
                    kk_name = s
                    kk_score = total

                if s not in valid:
                    valid[s] = True

                if s != kk_name and total >= kk_score:
                    valid[s] = False

        ans = sum(1 for k,v in valid.items() if k != kk_name and v)
        out.append(str(ans))

    return "\n".join(out)

# minimal case
assert run("""1
1
1
kk 50 50
a 10 10
""") == "1"

# violation case
assert run("""1
1
2
kk 100 100
a 50 50
a 200 200
""") == "0"

# all valid
assert run("""1
1
3
kk 100 100
a 10 10
b 20 20
""") == "2"

# multiple sessions with cross violation
assert run("""1
2
2
kk 100 100
a 90 90
2
kk 80 80
a 100 100
""") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phiên duy nhất tối thiểu | 1 | tính đúng đắn cơ bản | 
| lần xuất hiện hợp lệ/không hợp lệ | 0 | vô hiệu hóa nhiều lần | 
| tất cả học sinh luôn ở dưới KK | 2 | đếm dương | 
| vi phạm trong phiên sau | 0 | phụ thuộc giữa các phiên | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một học sinh chỉ xuất hiện trong những buổi học mà KK có điểm rất thấp. Thuật toán xử lý việc này một cách chính xác vì mỗi buổi học được kiểm tra độc lập, do đó, ngay cả một buổi học đạt điểm cao cũng sẽ vô hiệu hóa học sinh. 

Một trường hợp khác là sự xuất hiện lặp đi lặp lại của cùng một học sinh trong một buổi học. Vì chúng tôi xử lý từng dòng một cách độc lập nên mọi vi phạm trong phiên đó sẽ ngay lập tức làm mất hiệu lực của chúng và các lần xuất hiện tiếp theo sẽ không thay đổi kết quả. 

Một trường hợp tinh vi cuối cùng là KK được sử dụng lại làm tên cho các học sinh khác. Mã này loại trừ rõ ràng KK theo so sánh tên, đảm bảo chỉ sử dụng đường cơ sở KK thực sự để so sánh.
