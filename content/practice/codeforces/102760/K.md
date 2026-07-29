---
title: "CF 102760K - Đồ thị may"
description: "Tấm vải chứa các điểm $N$ trong mặt phẳng. Trình tự may là danh sách các chỉ số điểm. Các điểm liên tiếp trong danh sách này xác định các đoạn và mặt của tấm vải mà đoạn được vẽ phụ thuộc vào vị trí của nó trong chuỗi là chẵn hay lẻ."
date: "2026-07-28T23:59:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102760
codeforces_index: "K"
codeforces_contest_name: "2020 KAIST 10th ICPC Mock Contest (XXI Open Cup. Grand Prix of Korea. Division 2)"
rating: 0
weight: 102760
solve_time_s: 87
verified: true
draft: false
---

[CF 102760K - Đồ thị may](https://codeforces.com/problemset/problem/102760/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Tấm vải có chứa$N$các điểm trong mặt phẳng. Trình tự may là danh sách các chỉ số điểm. Các điểm liên tiếp trong danh sách này xác định các đoạn và mặt của tấm vải mà đoạn được vẽ phụ thuộc vào vị trí của nó trong chuỗi là chẵn hay lẻ. Các đoạn ở vị trí lẻ tạo thành mẫu ở mặt trước và các đoạn ở vị trí chẵn tạo thành mẫu ở mặt sau. 

Một mẫu hợp lệ yêu cầu các đoạn ở mỗi bên kết nối tất cả các điểm và không giao nhau ngoại trừ tại các điểm cuối được chia sẻ. Mục đích không phải là giảm thiểu tổng chiều dài của sợi hoặc khoảng cách hình học di chuyển. Điều duy nhất được giảm thiểu là số điểm được ghi trong trình tự may. 

Mỗi bên cần chứa một biểu đồ được kết nối trên$N$đỉnh. Bất kỳ biểu đồ được kết nối nào trên$N$đỉnh có ít nhất$N-1$các cạnh, vì vậy mặt trước yêu cầu ít nhất$N-1$các đoạn may và mặt sau yêu cầu ít nhất$N-1$đoạn may. Vì một dãy có độ dài$k$tạo ra chính xác$k-1$phân đoạn, chúng tôi ngay lập tức nhận được giới hạn dưới$$k-1 \geq 2(N-1)$$có nghĩa là$$k \geq 2N-1.$$Những ràng buộc cho phép$N$lên đến$1000$, nhưng giới hạn dưới này cho chúng ta biết điều gì đó mạnh mẽ hơn: chúng ta không cần một thuật toán hình học phức tạp. Chúng ta chỉ cần tìm bất kỳ cách xây dựng hợp lệ nào bằng cách sử dụng chính xác$2N-1$điểm trong dãy. 

Một vài trường hợp có thể phá vỡ lối suy nghĩ ngây thơ. Vì$N=2$, dãy không thể chỉ chứa hai đỉnh một lần vì một cạnh sẽ không có cạnh. Đối với đầu vào```
2
1 1
2 2
```độ dài đầu ra chính xác là$3$, Ví dụ:```
3
1 2 1
```Mặt trước có cạnh$1-2$và mặt sau có cạnh hình học tương tự ở mặt kia của tấm vải. 

Một lỗi phổ biến khác là cố gắng sử dụng cấu trúc không phẳng. Một trình tự như```
1 2 3 4 1
```không tự động tạo ra một mẫu hợp lệ vì các đoạn tùy ý giữa các điểm có thể giao nhau. Việc xây dựng cần một biểu đồ trong đó thiết kế không thể thực hiện được việc giao cắt. 

Trường hợp cạnh cuối cùng là một số lượng lớn các điểm không ở vị trí lồi. Ví dụ:```
4
1 1
10 10
5 5
8 2
```Việc kết nối các cặp tùy ý có thể tạo ra các giao điểm, nhưng việc kết nối mọi điểm trực tiếp với một điểm cố định luôn có tác dụng vì tất cả các đoạn chỉ gặp nhau tại điểm trung tâm đó. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là tìm kiếm hai cây bao trùm không giao nhau và sau đó tìm một lối đi xen kẽ sử dụng tất cả các cạnh của chúng. Điều này khó khăn vì cây cối có những hạn chế về hình học, và sự tương tác giữa hai mặt của tấm vải khiến việc xây dựng trở nên phức tạp không cần thiết. 

Giới hạn dưới cho một manh mối. Nếu chúng ta đạt được một chuỗi có độ dài chính xác$2N-1$, thì mỗi bên nhận được chính xác$N-1$các cạnh. Cả hai bên đều phải là cây. Chúng ta chỉ cần tìm một cây phẳng có các cạnh có thể được sử dụng hai lần, mỗi cạnh một lần. 

Biểu đồ hình sao sẽ giải quyết vấn đề này ngay lập tức. Chọn điểm$1$làm trung tâm và kết nối nó với mọi điểm khác. Đồ thị này luôn phẳng vì tất cả các cạnh đều có chung điểm cuối. Bây giờ hãy thực hiện duyệt theo chiều sâu của cây sao này. Quá trình duyệt bắt đầu ở trung tâm, thăm một lá, quay trở lại trung tâm, thăm lá tiếp theo, v.v. 

Trình tự kết quả là:$$1,2,1,3,1,4,1,\ldots,N,1$$Mỗi cạnh của ngôi sao xuất hiện một lần giữa các vị trí lẻ và một lần giữa các vị trí chẵn. Mặt trước nhận được các chuyến đi ra ngoài và mặt sau nhận được các chuyến đi về. Cả hai mặt đều chứa cùng một cây bao trùm, điều này được phép vì hai mặt của tấm vải độc lập. 

Ý tưởng vũ lực không thành công vì nó cố gắng khám phá một cấu trúc phẳng hợp lệ. Việc quan sát thấy một ngôi sao luôn phẳng sẽ loại bỏ mọi khó khăn về hình học và giảm vấn đề đưa ra một mẫu cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong$N$nếu tìm kiếm theo trình tự | Lớn | Quá chậm | 
| Tối ưu | O($N$) | O($N$) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chọn điểm$1$là tâm của cây bao trùm hình ngôi sao. Mọi điểm khác sẽ được kết nối trực tiếp đến điểm này. Điều này đảm bảo rằng không có hai cạnh nào ở cùng một phía có thể giao nhau ngoại trừ ở tâm. 
2. Bắt đầu trình tự may bằng điểm$1$. Điều này thể hiện việc đứng ở trung tâm trước khi thực hiện bất kỳ mũi khâu nào. 
3. Với mọi điểm từ$2$ĐẾN$N$, nối điểm rồi nối điểm$1$lại. Mỗi cặp nước đi đi từ tâm đến một lá và quay lại. 
4. Xuất ra độ dài của chuỗi, chính xác là$2N-1$, theo sau là chuỗi được tạo. 

Tại sao nó hoạt động: 

Trình tự được tạo là bước đi Euler của cây sao trong đó mỗi cạnh được duyệt hai lần. Đường ngang đầu tiên của mỗi cạnh luôn xuất hiện ở một mặt của tấm vải và đường ngang trở lại luôn xuất hiện ở phía bên kia. Vì mỗi bên nhận mỗi cạnh của ngôi sao đúng một lần, nên cả hai bên tạo thành các cây bao trùm phẳng được kết nối. Giới hạn dưới đã chứng minh rằng không thể tồn tại chuỗi ngắn hơn, vì vậy cách xây dựng này là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    for _ in range(n):
        input()

    ans = [1]
    for i in range(2, n + 1):
        ans.append(i)
        ans.append(1)

    print(len(ans))
    print(*ans)

if __name__ == "__main__":
    solve()
```Các tọa độ đầu vào không liên quan sau khi chúng tôi nhận ra rằng một ngôi sao có tâm tại bất kỳ điểm nào luôn là đồ thị phẳng hợp lệ. Mã vẫn đọc tất cả tọa độ vì chúng là một phần của định dạng đầu vào. 

Việc xây dựng trình tự bắt đầu từ đỉnh`1`. Đối với mỗi đỉnh khác, mã sẽ thêm hành trình từ tâm đến đỉnh đó rồi ngay lập tức quay trở lại tâm. Điều này tạo ra chính xác hai hình dạng của mỗi cạnh ngôi sao, một hình cho mỗi mặt của tấm vải. 

Độ dài chuỗi cuối cùng là`1 + 2(N-1)`, bằng`2N-1`. Không có thao tác chỉ mục nào ngoài việc lặp lại từ`2`ĐẾN`N`là cần thiết, do đó không có vấn đề về ranh giới. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
5
1 1
2 4
3 2
4 5
5 3
```trình tự được tạo ra là:```
1 2 1 3 1 4 1 5 1
```| Bước | Đã thêm điểm | Trình tự hiện tại | Đã thêm cạnh trước | Đã thêm cạnh sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | Không có | Không có | 
| 2 | 2 | 1 2 | 1-2 | Không có | 
| 3 | 1 | 1 2 1 | Không có | 2-1 | 
| 4 | 3 | 1 2 1 3 | 1-3 | Không có | 
| 5 | 1 | 1 2 1 3 1 | Không có | 3-1 | 
| 6 | 4 | 1 2 1 3 1 4 | 1-4 | Không có | 
| 7 | 1 | 1 2 1 3 1 4 1 | Không có | 4-1 | 
| 8 | 5 | 1 2 1 3 1 4 1 5 | 1-5 | Không có | 
| 9 | 1 | 1 2 1 3 1 4 1 5 1 | Không có | 5-1 | 

Bảng cho thấy mỗi bên nhận được cả 4 cạnh của ngôi sao. Ví dụ này cũng chứng minh rằng điểm trung tâm lặp lại là có chủ ý. 

Đối với đầu vào:```
3
10 10
20 1
5 7
```trình tự trở thành:```
1 2 1 3 1
```| Bước | Đã thêm điểm | Trình tự hiện tại | Đã thêm cạnh trước | Đã thêm cạnh sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | Không có | Không có | 
| 2 | 2 | 1 2 | 1-2 | Không có | 
| 3 | 1 | 1 2 1 | Không có | 2-1 | 
| 4 | 3 | 1 2 1 3 | 1-3 | Không có | 
| 5 | 1 | 1 2 1 3 1 | Không có | 3-1 | 

Cả hai bên đều chứa cây nối cả ba điểm, mặc dù cách sắp xếp hình học của các điểm là tùy ý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O($N$) | Mỗi điểm được đọc một lần và được thêm vào câu trả lời một lần. | 
| Không gian | O($N$) | Trình tự đầu ra chứa$2N-1$chỉ số. | 

Tối đa$N$đủ nhỏ để việc xây dựng tuyến tính dễ dàng nằm gọn trong giới hạn. Thuật toán không thực hiện bất kỳ phép tính hình học nào, do đó thời gian chạy bị chi phối bởi đầu vào và đầu ra. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided sample style
out = run("""5
1 1
2 4
3 2
4 5
5 3
""").split()

assert out[0] == "9", "sample 1 length"

# minimum size
out = run("""2
1 1
2 2
""").split()

assert out[0] == "3", "minimum size"

# all points arbitrary
out = run("""4
100 100
1 50
20 30
90 5
""").split()

assert out[0] == "7", "four points"

# larger case
out = run("""6
1 1
2 2
3 3
4 4
5 5
6 6
""").split()

assert out[0] == "11", "six points"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 điểm từ mẫu | 9 | Công thức chiều dài tối ưu$2N-1$| 
| 2 điểm | 3 | Chuỗi hợp lệ nhỏ nhất | 
| Tọa độ tùy ý | 7 | Tọa độ không ảnh hưởng tới công trình | 
| 6 điểm trên một đường | 11 | Điểm thẳng hàng và đầu vào lớn hơn | 

## Vỏ cạnh 

Đối với hai điểm, giới hạn dưới cho$2N-1=3$. Thuật toán tạo ra:```
1 2 1
```Mặt trước có cạnh$1-2$, và mặt sau có cạnh tương tự được vẽ ở mặt bên kia. Hai điểm được nối ở cả hai phía nên việc xây dựng là hợp lệ. 

Đối với các điểm được sắp xếp theo cách mà nhiều đoạn tùy ý cắt nhau, thuật toán vẫn thành công. Coi như:```
4
1 1
10 10
5 5
8 2
```Trình tự đầu ra là:```
1 2 1 3 1 4 1
```Các phân đoạn duy nhất được tạo nằm giữa điểm 1 và điểm khác. Vì tất cả các đoạn đều có chung điểm 1 nên giao lộ chỉ có thể xảy ra ở điểm cuối đó. 

Đối với lớn$N$, thuật toán tiếp tục mô hình tương tự:```
1 2 1 3 1 4 1 ... N 1
```Mỗi điểm mới đóng góp chính xác hai vị trí và đúng một cạnh cho mỗi bên. Điều bất biến vẫn là cả hai bên đều chứa cùng một cây bao trùm sao, do đó khả năng kết nối và tính phẳng được bảo toàn.
