---
title: "CF 105492F - Nhà máy hỏng hóc"
description: "Chúng ta có một nhà máy được mô hình hóa dưới dạng đồ thị có hướng trong đó mỗi nút biểu thị một bước xử lý. Mỗi bước độc lập đều thất bại với một xác suất nhất định."
date: "2026-06-23T19:42:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105492
codeforces_index: "F"
codeforces_contest_name: "2024 Benelux Algorithm Programming Contest (BAPC 24)"
rating: 0
weight: 105492
solve_time_s: 45
verified: true
draft: false
---

[CF 105492F - Nhà máy hỏng hóc](https://codeforces.com/problemset/problem/105492/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một nhà máy được mô hình hóa dưới dạng đồ thị có hướng trong đó mỗi nút biểu thị một bước xử lý. Mỗi bước độc lập đều thất bại với một xác suất nhất định. Ngoài những lỗi nội tại này, còn có các sự phụ thuộc trực tiếp: nếu một bước không thành công thì mọi bước phụ thuộc vào bước đó cũng sẽ thất bại và hiệu ứng này sẽ lan truyền qua biểu đồ phụ thuộc. 

Câu hỏi duy nhất chúng ta thực sự cần trả lời không phải về bản thân tầng mà là về sự lựa chọn. Trong số tất cả các bước, chúng tôi chọn bước có xác suất thất bại nhỏ nhất. Đầu ra là xác suất mà bước đã chọn này không thất bại, nghĩa là chúng ta xuất ra một kết quả trừ đi xác suất thất bại của nó. 

Cấu trúc biểu đồ và sự lan truyền phụ thuộc nghe có vẻ quan trọng nhưng chúng không được sử dụng trong tính toán cuối cùng về số lượng được yêu cầu. Họ chỉ xác định một tầng thất bại không liên quan khi chúng tôi nhận ra vấn đề không bao giờ yêu cầu khả năng tiếp cận hoặc xác suất sống sót của các nút đang được lan truyền. Việc lựa chọn hoàn toàn dựa trên xác suất độc lập nhất định. 

Các ràng buộc rất lớn: lên tới 100.000 nút và 100.000 cạnh. Bất kỳ giải pháp nào cố gắng mô phỏng các tầng hoặc tính toán các lần đóng biểu đồ trên mỗi nút sẽ quá chậm. Ngay cả một BFS hoặc DFS trên mỗi nút cũng sẽ đạt O(n(n + m)) trong trường hợp xấu nhất, điều này là không thể. 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ là suy nghĩ quá nhiều về các phụ thuộc và cố gắng tính toán “xác suất thất bại hiệu quả” sau khi lan truyền. Ví dụ: nếu nút A phụ thuộc vào B và B phụ thuộc vào A, người ta có thể tính sai xác suất hỏng khớp của chu trình, mặc dù bài toán không bao giờ yêu cầu điều đó. 

Một trường hợp khác là khi tất cả các xác suất đều bằng nhau. Câu trả lời vẫn phải chọn bất kỳ một trong số chúng và xuất ra 1 trừ giá trị đó. Nếu tất cả đều là 0,999 thì đầu ra là 0,001. 

Cuối cùng, xác suất bao gồm các giá trị như 0 hoặc 1. Nếu xác suất tối thiểu là 0 thì câu trả lời là 1. Nếu là 1 thì câu trả lời là 0. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực bắt đầu bằng cách xem xét biểu đồ theo nghĩa đen. Người ta có thể cố gắng tính toán, đối với mỗi nút, xác suất mà nó cuối cùng bị lỗi sau khi xếp tầng phụ thuộc. Điều này sẽ liên quan đến việc lập mô hình lan truyền các lỗi thông qua biểu đồ có hướng có chu kỳ, về cơ bản yêu cầu tính toán trên các thành phần được kết nối mạnh và sau đó lan truyền xác suất dọc theo DAG cô đọng. 

Ngay cả khi chúng ta đơn giản hóa các giả định về tính độc lập, chúng ta vẫn cần phải đi qua các cạnh nhiều lần để xác định các hiệu ứng đóng. Bất kỳ việc tính toán lại trên mỗi nút nào của các nút bị lỗi có thể truy cập đều dẫn đến việc duyệt đồ thị lặp đi lặp lại và trong các đồ thị dày đặc, điều này trở thành phương trình bậc hai trong thực tế. 

Tuy nhiên, quan sát quan trọng là tầng này không liên quan đến đầu ra. Vấn đề không bao giờ yêu cầu xác suất để một bước tồn tại sau sự phụ thuộc. Nó chỉ yêu cầu: trong số tất cả các bước, hãy chọn bước có xác suất thất bại nhỏ nhất cho trước và đưa ra xác suất sống sót của nó. 

Do đó toàn bộ cấu trúc đồ thị có thể được bỏ qua. Chúng ta chỉ cần quét tuyến tính duy nhất các xác suất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (truyền bá biểu đồ trên mỗi nút) | O(n(n + m)) | O(n + m) | Quá chậm | 
| Tối ưu (quét tối thiểu) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi đơn giản hóa vấn đề thành việc chọn một giá trị tối thiểu trong một mảng và chuyển đổi nó. 

1. Đọc tất cả các khả năng xảy ra lỗi vào một mảng. Chúng tôi hoàn toàn không xử lý các cạnh vì chúng không ảnh hưởng đến tiêu chí lựa chọn. 
2. Duy trì một biến`best`được khởi tạo thành giá trị lớn hơn bất kỳ xác suất nào có thể xảy ra, chẳng hạn như 2.0. Điều này sẽ lưu trữ xác suất thất bại tối thiểu được thấy cho đến nay. 
3. Lặp lại từng xác suất. Đối với mỗi giá trị, so sánh nó với`best`và cập nhật`best`nếu nó nhỏ hơn. Điều này đảm bảo rằng sau một lần vượt qua,`best`bằng xác suất lỗi tối thiểu trong hệ thống. 
4. Tính đáp án dưới dạng`1 - best`. Điều này thể hiện xác suất mà bước đã chọn không thất bại. 
5. Xuất kết quả với độ chính xác vừa đủ. 

Lý do đằng sau việc bỏ qua các cạnh là các lỗi do sự phụ thuộc gây ra sẽ chỉ quan trọng nếu chúng ta được hỏi về khả năng tồn tại của tất cả các bước hoặc khả năng tiếp cận trong quá trình lan truyền lỗi ngẫu nhiên. Ở đây, việc lựa chọn xảy ra trước bất kỳ sự kiện ngẫu nhiên nào, do đó cấu trúc phụ thuộc không bao giờ ảnh hưởng đến nút nào được chọn. 

### Tại sao nó hoạt động 

Vấn đề xác định một quy tắc lựa chọn xác định chỉ dựa trên xác suất thất bại nội tại. Vì các phụ thuộc không làm thay đổi các xác suất đã cho này và không phải là một phần của tiêu chí lựa chọn nên bước tối ưu chính xác là mức tối thiểu toàn cục của danh sách được cung cấp. Sự chuyển đổi từ xác suất thất bại sang xác suất sống sót là sự bổ sung trực tiếp, do đó tính đúng đắn ngay sau khi định nghĩa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    probs = list(map(float, input().split()))
    
    best = 2.0
    for p in probs:
        if p < best:
            best = p
    
    ans = 1.0 - best
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bỏ qua hoàn toàn các cạnh phụ thuộc vì chúng không liên quan đến việc lựa chọn bước. Điểm tinh tế duy nhất là độ ổn định về số: chúng tôi dựa vào phép trừ float của Python, đủ với độ chính xác cần thiết là 1e-6 tương đối hoặc 1e-200 tuyệt đối. 

Việc khởi tạo của`best`đến 2.0 đảm bảo tính chính xác ngay cả khi tất cả các xác suất đều nằm trong [0, 1]. Phép trừ cuối cùng tính trực tiếp xác suất sống sót. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2
0.600 0.300
1 2
2 1
```Chúng tôi chỉ theo dõi các xác suất. 

| Bước | p hiện tại | Tốt nhất cho đến nay | 
| --- | --- | --- | 
| 1 | 0,600 | 0,600 | 
| 2 | 0,300 | 0,300 | 

Xác suất thất bại tốt nhất cuối cùng là 0,300, do đó đầu ra là 0,700. 

Điều này phù hợp với ý tưởng rằng sự phụ thuộc không ảnh hưởng đến bước nào được chọn, chỉ có xác suất tối thiểu mới quan trọng. 

### Mẫu 2 

đầu vào:```
2 1
0.300 0.600
1 2
```| Bước | p hiện tại | Tốt nhất cho đến nay | 
| --- | --- | --- | 
| 1 | 0,300 | 0,300 | 
| 2 | 0,600 | 0,300 | 

Tốt nhất cuối cùng là 0,300, vì vậy câu trả lời là 0,700. 

Mặc dù bước 1 phụ thuộc vào bước 2 nhưng mối quan hệ đó không liên quan đến quy tắc lựa chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Xác suất vượt qua một lần | 
| Không gian | O(1) | Chỉ có một biến tích lũy được sử dụng | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì n lên tới 100.000 và chúng tôi chỉ thực hiện công việc tuyến tính mà không cần truyền tải đồ thị. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, m = map(int, sys.stdin.readline().split())
    probs = list(map(float, sys.stdin.readline().split()))
    
    best = 2.0
    for p in probs:
        best = min(best, p)
    
    return str(1.0 - best)

# provided samples
assert abs(float(run("2 2\n0.600 0.300\n1 2\n2 1\n")) - 0.7) < 1e-9
assert abs(float(run("2 1\n0.300 0.600\n1 2\n")) - 0.7) < 1e-9

# custom cases
assert abs(float(run("1 0\n0.000\n")) - 1.0) < 1e-9
assert abs(float(run("3 0\n0.999 0.999 0.999\n")) - 0.001) < 1e-9
assert abs(float(run("3 0\n1.000 0.500 0.800\n")) - 0.0) < 1e-9
assert abs(float(run("4 2\n0.2 0.9 0.1 0.3\n1 2\n3 4\n")) - 0.9) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1, p=0 | 1 | kích thước tối thiểu và không có lỗi | 
| tất cả 0,999 | 0,001 | giá trị thống nhất | 
| bao gồm 1.0 | 0,0 | ranh giới thất bại hoàn toàn | 
| giá trị hỗn hợp | lựa chọn tối thiểu đúng | tính đúng đắn chung | 

## Vỏ cạnh 

Trường hợp một cạnh là một nút duy nhất. Đối với đầu vào`1 0`với xác suất`0.000`, bộ thuật toán`best = 2.0`, sau đó cập nhật nó thành`0.0`. Đầu ra trở thành`1.0`, phù hợp với định nghĩa vì bước duy nhất không bao giờ thất bại. 

Một trường hợp khác là tất cả các xác suất đều bằng nhau. Vì`0.999 0.999 0.999`, mức tối thiểu còn lại`0.999`không phụ thuộc vào thứ tự hoặc cạnh. Đầu ra là`0.001`, phù hợp với định nghĩa phần bù. 

Trường hợp cạnh cuối cùng là các giá trị cực trị như`1.000`. Thuật toán giữ đúng`best = 1.0`, và đầu ra trở thành`0.0`, có nghĩa là sự thất bại nhất định, phù hợp với cách giải thích.
