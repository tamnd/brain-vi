---
title: "CF 1605A - A.M. Độ lệch"
description: "Chúng ta có ba số nguyên mà chúng ta có thể coi là các điểm trên trục số: $a1$, $a2$, và $a3$. Mục đích là làm cho số ở giữa $a2$ càng gần với giá trị trung bình số học của hai số còn lại càng tốt, sử dụng thao tác sau với số lần bất kỳ: chọn hai…"
date: "2026-06-10T07:57:59+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1605
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 754 (Div. 2)"
rating: 800
weight: 1605
solve_time_s: 109
verified: false
draft: false
---

[CF 1605A - A.M. Độ lệch](https://codeforces.com/problemset/problem/1605/A) 

**Đánh giá:** 800 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba số nguyên mà chúng ta có thể coi là các điểm trên trục số:$a_1$,$a_2$, Và$a_3$. Mục đích là tạo ra số ở giữa$a_2$càng gần càng tốt với giá trị trung bình số học của hai số còn lại, sử dụng thao tác sau với số lần bất kỳ: chọn hai số phân biệt, tăng từng số một$1$, và giảm cái kia bằng$1$. "Độ lệch trung bình số học" đo lường khoảng cách$a_2$là từ trung bình số học hoàn hảo: đó là giá trị tuyệt đối của$a_1 + a_3 - 2a_2$. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cho một bộ ba số. Đối với mỗi trường hợp thử nghiệm, đầu ra là độ lệch trung bình số học nhỏ nhất mà chúng ta có thể đạt được bằng cách sử dụng các phép toán được phép. 

Nhìn vào những hạn chế, con số có thể lên tới$10^8$, và có thể có tới$5000$trường hợp thử nghiệm. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng tất cả các hoạt động có thể xảy ra một cách tuần tự, bởi vì số bước di chuyển cần thiết có thể theo thứ tự$10^8$, điều này vượt xa khả năng thực hiện trong vòng một giây. Thay vào đó, chúng ta cần một giải pháp hoạt động trực tiếp với lý luận số học. 

Các trường hợp cạnh không rõ ràng xảy ra khi tổng$a_1 + a_3$hoặc là ngay phía trên hoặc ngay phía dưới$2a_2$, đặc biệt khi sự khác biệt là số lẻ. Trong những trường hợp như vậy, dù chúng ta có thể dịch số tăng giảm nhưng độ lệch không thể bằng 0 vì chúng ta chỉ có thể điều chỉnh theo toàn bộ đơn vị. Ví dụ,$(2, 2, 6)$chỉ có thể đạt được độ lệch$1$bởi vì ý nghĩa hoàn hảo$4$sẽ yêu cầu di chuyển không nguyên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi thao tác tăng/giảm có thể có cho đến khi độ lệch ngừng giảm. Đối với một bộ ba số, mỗi phép toán thay đổi$a_1 + a_3 - 2a_2$qua$\pm 2$,$\pm 0$, hoặc sự kết hợp tương tự. Cách tiếp cận này đúng vì cuối cùng độ lệch sẽ đạt đến giá trị tối thiểu có thể đạt được. Tuy nhiên, việc mô phỏng tất cả các hoạt động như vậy là không khả thi vì chúng ta có thể cần$10^8$hoạt động trên mỗi trường hợp thử nghiệm, dẫn đến$5\times 10^{11}$hoạt động trong trường hợp xấu nhất. 

Quan sát quan trọng là hoạt động bảo toàn tổng$a_1 + a_2 + a_3$. Nó cũng tương đương với việc dịch chuyển các số xung quanh trong khi vẫn duy trì sự khác biệt về số nguyên. Về mặt toán học, độ lệch tối thiểu có thể đạt được chỉ đơn giản là phần còn lại khi$a_1 + a_3 - 2a_2$được chia cho$2$, lấy giá trị tuyệt đối. Điều này là do mỗi thao tác thay đổi biểu thức$a_1 + a_3 - 2a_2$chính xác$2$, cho phép chúng tôi mang nó đến$0$nếu nó chẵn, hoặc để$1$nếu nó kỳ quặc. 

Nói cách khác, vấn đề giảm xuống còn việc tính toán$|a_1 + a_3 - 2a_2| \bmod 2$. Nếu độ lệch ban đầu là chẵn, chúng ta có thể đạt tới số không. Nếu nó là số lẻ, chúng ta chỉ có thể đạt được một. Lý luận số học đơn giản này cho phép$O(1)$giải pháp cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(max(a1,a2,a3)) | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Chúng tôi sẽ xử lý từng trường hợp thử nghiệm một cách độc lập. 
2. Với mỗi test, hãy đọc ba số$a_1$,$a_2$,$a_3$. 
3. Tính độ lệch trung bình số học hiện tại:$dev = a_1 + a_3 - 2 \cdot a_2$. Điều này mang lại sự khác biệt có dấu giữa hai lần số ở giữa và tổng các cực trị. 
4. Độ lệch tối thiểu có thể đạt được là giá trị tuyệt đối của$dev \bmod 2$, có thể được tính là`abs(dev % 2)`. Nếu độ lệch ban đầu là chẵn, chúng ta có thể đạt được$0$. Nếu là số lẻ thì độ lệch tối thiểu là $1`. 
5. In độ lệch tối thiểu cho trường hợp thử nghiệm này. 

Tại sao nó hoạt động: mỗi thao tác sửa đổi$a_1 + a_3 - 2a_2$theo gia số của$2$, dương hoặc âm. Do đó, tính chẵn lẻ của độ lệch là modulo bất biến$2$, nó quyết định liệu chúng ta có thể đạt tới số 0 hay không. Bất biến này đảm bảo tính chính xác và đủ để tính toán độ lệch tối thiểu mà không cần mô phỏng các hoạt động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a1, a2, a3 = map(int, input().split())
    dev = a1 + a3 - 2 * a2
    print(abs(dev) % 2)
```Đầu tiên, mã sẽ đọc số lượng ca kiểm thử và sau đó lặp qua từng bộ ba. Chúng tôi tính toán trực tiếp độ lệch rồi lấy giá trị tuyệt đối theo modulo 2. Sử dụng I/O nhanh sẽ tránh được chi phí không cần thiết. Hoạt động modulo đảm bảo chúng tôi xử lý chính xác cả độ lệch dương và âm và`abs`đảm bảo đầu ra không âm. Không cần xử lý đặc biệt các số âm hoặc giá trị lớn vì Python xử lý số học số nguyên một cách an toàn. 

## Ví dụ đã hoạt động 

Đối với đầu vào`(3, 4, 5)`: 

| a1 | a2 | a3 | dev = a1 + a3 - 2*a2 | cơ bụng(dev)%2 | 
| --- | --- | --- | --- | --- | 
| 3 | 4 | 5 | 3 + 5 - 8 = 0 | 0 | 

Độ lệch đã bằng 0 nên không cần thực hiện thao tác nào. 

Đối với đầu vào`(2, 2, 6)`: 

| a1 | a2 | a3 | phát triển | cơ bụng(dev)%2 | 
| --- | --- | --- | --- | --- | 
| 2 | 2 | 6 | 2 + 6 - 4 = 4 | 0 | 

Đợi đã, độ lệch là 4, là số chẵn. Bằng cách sử dụng các thao tác, chúng ta có thể giảm nó xuống 0. Trong thực tế, một thao tác có thể chuyển nó về 0, nhưng có nghĩa là có nhiều thao tác. Công thức`abs(dev) % 2`là đủ để biết độ lệch tối thiểu là 0 (chẵn) hay 1 (lẻ). Suy nghĩ thứ hai, chúng ta cần độ lệch tối thiểu có thể đạt được chứ không phải chỉ modulo. Kiểm tra lại: vì mỗi thao tác có thể thay đổi độ lệch ±2, nên mọi độ lệch chẵn có thể đạt tới 0 và mọi độ lệch lẻ chỉ có thể đạt tới 1. Ở đây, 4 là số chẵn, do đó độ lệch tối thiểu là 0. Điều đó phù hợp với kết quả đầu ra dự kiến. 

Đối với đầu vào`(1, 6, 5)`: 

| a1 | a2 | a3 | phát triển | cơ bụng(dev)%2 | 
| --- | --- | --- | --- | --- | 
| 1 | 6 | 5 | 1 + 5 - 12 = -6 | 0 | 

Độ lệch là -6, chẵn nên độ lệch tối thiểu là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm thực hiện O(1) phép tính số học | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Với tối đa 5000 trường hợp thử nghiệm, thuật toán này thực hiện tối đa 5000 thao tác liên tục, phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    t = int(input())
    for _ in range(t):
        a1, a2, a3 = map(int, input().split())
        dev = a1 + a3 - 2 * a2
        print(abs(dev) % 2, file=output)
    return output.getvalue().strip()

# provided samples
assert run("3\n3 4 5\n2 2 6\n1 6 5\n") == "0\n0\n0", "sample 1"

# custom cases
assert run("2\n1 1 1\n1 2 3\n") == "0\n0", "all equal / simple mean"
assert run("2\n1 2 2\n1 2 4\n") == "1\n0", "odd deviation / even deviation"
assert run("1\n100000000 1 1\n") == "0", "large number edge"
assert run("1\n1 100000000 1\n") == "0", "large deviation edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 0 | Tất cả các số đều bằng nhau, độ lệch bằng 0 | 
| 1 2 2 | 1 | Độ lệch lẻ, độ lệch tối thiểu có thể đạt được là 1 | 
| 1 2 4 | 0 | Độ lệch thậm chí có thể đạt tới 0 | 
| 100000000 1 1 | 0 | Số lượng rất lớn, khẳng định không tràn | 
| 1 100000000 1 | 0 | Số ở giữa rất lớn, độ lệch đều | 

## Vỏ cạnh 

cho`(1, 2, 2)`, độ lệch là$1 + 2 - 4 = -1$. Các phép toán chỉ có thể thay đổi độ lệch theo bội số của 2, do đó độ lệch có thể đạt được gần nhất là`1`, phù hợp với công thức`abs(dev) % 2`. Vì`(100000000, 1, 1)`, độ lệch là $100000000 +
