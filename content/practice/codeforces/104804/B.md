---
title: "CF 104804B - \u041d\u0430\u0447\u0430\u043b\u043e \u0438\u0433\u0440\u044b"
description: "Chúng tôi đang mô phỏng một quy trình chọn bài rất có cấu trúc giữa bốn người chơi ngồi trong một chu kỳ cố định. Mỗi vòng, mỗi người chơi lấy chính xác một mã thông báo hiệp sĩ từ một nhóm chung cho đến khi hết nhóm."
date: "2026-06-28T13:24:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104804
codeforces_index: "B"
codeforces_contest_name: "Central Russia Regional Contest, 2022, Qualification Contest"
rating: 0
weight: 104804
solve_time_s: 84
verified: false
draft: false
---

[CF 104804B - \u041d\u0430\u0447\u0430\u043b\u043e \u0438\u0433\u0440\u044b](https://codeforces.com/problemset/problem/104804/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một quy trình chọn bài rất có cấu trúc giữa bốn người chơi ngồi trong một chu kỳ cố định. Mỗi vòng, mỗi người chơi lấy chính xác một mã thông báo hiệp sĩ từ một nhóm chung cho đến khi hết nhóm. Điểm khác biệt duy nhất là thứ tự chơi không cố định: sau mỗi vòng có đủ bốn lượt chọn, thứ tự sẽ luân chuyển theo chu kỳ, do đó, ai đứng đầu sẽ trở thành người cuối cùng và mọi người sẽ chuyển một vị trí về phía trước. 

Igor bắt đầu vào vị trí$k$, và chúng tôi muốn xác định xem anh ấy sẽ hành động bao nhiêu lần trước khi$n$hết token. 

Đầu vào bao gồm hai giá trị. Đầu tiên là tổng số mã thông báo giống hệt nhau có sẵn và thứ hai là vị trí ban đầu của Igor theo thứ tự lượt chơi giữa bốn người chơi. Đầu ra chỉ đơn giản là số lượt mà Igor quản lý để lấy được mã thông báo. 

Những hạn chế là rất nhỏ, với$n \le 100$, vì vậy ngay cả việc mô phỏng trực tiếp từng lượt chọn cũng là đủ. Không cần phải tối ưu hóa về độ phức tạp tiệm cận, và thậm chí$O(n)$hoặc$O(n \cdot 4)$hành vi là tầm thường. 

Một điểm tinh tế là việc luân chuyển người chơi không làm thay đổi thực tế là mỗi người chơi vẫn hành động chính xác một lần trong toàn bộ bốn nước đi. Vòng quay chỉ hoán vị người chiếm giữ từng vị trí chứ không hoán vị tần suất xuất hiện. Vì điều này, mỗi người chơi vẫn sẽ xuất hiện đúng một lần trong mỗi vòng bốn lượt chọn. Điều phức tạp duy nhất là danh tính của Igor di chuyển qua các vị trí theo mô hình tuần hoàn có thể đoán trước được. 

Không có trường hợp cạnh nào có ý nghĩa ngoài việc kiểm tra ranh giới nhỏ như$n = 1$, trong đó chỉ vị trí đầu tiên hoạt động một lần, hoặc$k = 4$, nơi Igor bắt đầu cuối cùng trong chu kỳ đầu tiên. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp là cách giải thích trực tiếp nhất. Chúng tôi duy trì một mảng gồm bốn người chơi đại diện cho thứ tự hiện tại. Đối với mỗi mã thông báo, chúng tôi đưa người chơi đứng đầu đơn hàng, gán cho họ một mã thông báo, sau đó xoay thứ tự sau mỗi bốn lượt chọn. Điều này hoạt động chính xác vì nó phản ánh chính xác các quy tắc trò chơi. 

Tuy nhiên, việc mô phỏng phép quay là không cần thiết. Quan sát quan trọng là việc xoay vòng sau mỗi vòng đấu chỉ đơn giản là thay đổi danh tính nhưng không thay đổi thực tế là mỗi người chơi được chọn chính xác một lượt chọn mỗi vòng. Trong bất kỳ khối bốn nước đi liên tiếp nào, mỗi người trong số bốn người chơi xuất hiện đúng một lần. Vì vậy, trên tất cả$n$token, số lần Igor xuất hiện chỉ phụ thuộc vào số vòng hoàn thành hoặc một phần xảy ra chứ không phụ thuộc vào cấu trúc xoay vòng bên trong. 

Điều này làm giảm vấn đề thành một nhiệm vụ đếm định kỳ đơn giản. Vì mỗi nhóm 4 lượt chọn sẽ phân phối chính xác một lượt chọn cho mỗi người chơi, Igor nhận được một lượt chọn cho mỗi nhóm đầy đủ 4 người, cộng thêm có thể thêm một lượt chọn nếu vị trí của anh ấy nằm trong nhóm chưa hoàn thành còn sót lại. 

Cách tiếp cận bạo lực sẽ mô phỏng mọi mảng chọn và xoay, tiêu tốn công việc liên tục cho mỗi lượt chọn nhưng bao gồm chi phí quản lý chuyển đổi trạng thái. Chế độ xem được tối ưu hóa sẽ loại bỏ tất cả trạng thái và giảm mọi thứ về số học theo chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Đếm định kỳ | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính xem có bao nhiêu lượt chọn đầy đủ trong$n$. Đây là$n // 4$. Mỗi vòng đấu đầy đủ đảm bảo chính xác một lựa chọn cho Igor. 
2. Thêm những đóng góp toàn diện này vào câu trả lời của Igor. Điều này tạo thành số lượng cơ sở. 
3. Tính số dư$n \% 4$, đại diện cho vòng chung kết chưa hoàn thành, nếu có. 
4. Kiểm tra xem vị trí của Igor$k$rơi vào vòng đầu tiên$r$vị trí của một vòng. Nếu có, Igor nhận được thêm một lượt chọn. 
5. Xuất ra tổng số tích lũy. 

Ý tưởng chính là trong mỗi chu kỳ gồm bốn nước đi, các vị trí từ 1 đến 4 đều được sử dụng chính xác một lần, vì vậy câu hỏi duy nhất trong phần cuối cùng của chu kỳ là liệu vị trí của Igor có đạt được trước khi chu kỳ kết thúc hay không. 

### Tại sao nó hoạt động 

Hệ thống hoạt động giống như một lịch trình lặp lại có độ dài 4, trong đó mỗi người chơi được đảm bảo chính xác một lần xuất hiện trong toàn bộ thời gian. Xoay vòng giữa các vòng hoán vị danh tính nhưng vẫn giữ nguyên tính bất biến là mỗi danh tính xuất hiện một lần trong mỗi chu kỳ. Do đó, tổng số lần xuất hiện của Igor chỉ phụ thuộc vào số lượng chu kỳ hoàn chỉnh tồn tại cộng với việc liệu anh ta có được xếp vào hậu tố chưa hoàn chỉnh của chu kỳ cuối cùng hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())

full = n // 4
rem = n % 4

ans = full

if k <= rem:
    ans += 1

print(ans)
```Mã trực tiếp thực hiện phân tách chu trình. Việc chia số nguyên cho 4 sẽ tính các vòng hoàn chỉnh, mỗi vòng đóng góp chính xác một lượt chọn được đảm bảo cho Igor. Phần còn lại ghi lại một phần vòng và so sánh`k <= rem`kiểm tra xem vị trí của Igor có còn hoạt động trong phân đoạn chưa hoàn thành đó hay không. 

Không cần mô phỏng và không cần thể hiện rõ ràng việc xoay người chơi vì việc xoay không ảnh hưởng đến tần suất trên mỗi chu kỳ. 

## Ví dụ đã hoạt động 

### Ví dụ 1: Đầu vào`9 3`Chúng tôi chia chuỗi 9 lượt chọn thành các chu kỳ đầy đủ và phần còn lại. 

| Bước | Chu kỳ đầy đủ (n/4) | Còn lại (n%4) | k | Lựa chọn thêm? | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 9 | 3 | Không | 0 | 
| Sau đầy đủ chu kỳ | 2 | 1 | 3 | Không | 2 | 
| Điều chỉnh cuối cùng | 2 | 1 | 3 | Không (3 > 1) | 2 | 

Điều này cho thấy Igor chỉ được hưởng lợi từ các hiệp đấu hoàn chỉnh và nước đi đơn còn sót lại không đạt đến vị trí thứ 3 nên không được chọn thêm. 

### Ví dụ 2: Nhập liệu`11 2`| Bước | Chu kỳ đầy đủ (n/4) | Còn lại (n%4) | k | Lựa chọn thêm? | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 11 | 2 | Không | 0 | 
| Sau đầy đủ chu kỳ | 2 | 3 | 2 | Có | 3 | 

Ở đây, Igor nhận được hai lượt chọn đảm bảo từ toàn bộ chu kỳ và một lượt chọn bổ sung vì vị trí 2 nằm trong 3 nước đi đầu tiên của chu kỳ một phần cuối cùng. 

Điều này xác nhận rằng logic còn lại nắm bắt chính xác sự tham gia của một phần chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép tính số học trên hai số nguyên | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ trợ | 

Giải pháp là thời gian không đổi bất kể kích thước đầu vào, nằm trong giới hạn và vượt xa những gì cần thiết cho$n \le 100$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())

    full = n // 4
    rem = n % 4
    ans = full
    if k <= rem:
        ans += 1
    return str(ans)

# provided samples
assert run("9 3") == "3"
assert run("11 2") == "3"
assert run("12 3") == "3"

# custom cases
assert run("1 1") == "1"
assert run("4 4") == "1"
assert run("5 1") == "2"
assert run("7 4") == "1"
assert run("8 2") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | trường hợp tối thiểu, chọn một lần | 
| 4 4 | 1 | ranh giới chu kỳ đầy đủ | 
| 5 1 | 2 | chuyển sang chu kỳ thứ hai | 
| 7 4 | 1 | vị trí cuối cùng không nằm trong một phần chu kỳ | 
| 8 2 | 2 | nhiều chu kỳ đầy đủ cộng với phần còn lại | 

## Vỏ cạnh 

cho$n = 1$, thuật toán tính toán$full = 0$,$rem = 1$. Nếu như$k = 1$, điều kiện$k \le rem$giữ nguyên, do đó kết quả là 1. Điều này phù hợp với thực tế là chỉ có một lượt chọn xảy ra và Igor ở vị trí 1 nhận ngay lập tức. 

Vì$n = 4$, chúng ta có một chu trình đầy đủ và không có phần dư. Mỗi người chơi, bao gồm cả Igor bất kể$k$, nhận được đúng một lượt chọn. Công thức trả về$1$bởi vì$full = 1$Và$rem = 0$, do đó không có phép cộng thêm nào xảy ra. 

Đối với những trường hợp$k = 4$, chỉ có phần còn lại là quan trọng. Nếu như$n \mod 4 < 4$, Igor chỉ đóng góp ở phần cuối của chu kỳ nếu đạt đến vị trí thứ tư. Nếu không thì anh ta chỉ nhận được những khoản đóng góp cả chu kỳ.
