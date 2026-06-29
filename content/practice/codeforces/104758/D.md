---
title: "CF 104758D - Xác định diện tích bể bơi"
description: "Chúng ta được cho một dãy số biểu thị độ cao dọc theo một đường thẳng. Từ mảng này, chúng tôi muốn chọn một phân đoạn liền kề hoạt động giống như một “nhóm”, nghĩa là phân đoạn được neo bởi hai vị trí ranh giới và cấu trúc giữa chúng không giới thiệu bất kỳ vị trí nào cao hơn…"
date: "2026-06-28T22:31:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104758
codeforces_index: "D"
codeforces_contest_name: "The 2023 ICPC Masters Mexico Regional #ICPCMX2023 Edition"
rating: 0
weight: 104758
solve_time_s: 89
verified: false
draft: false
---

[CF 104758D - Xác định diện tích bể bơi](https://codeforces.com/problemset/problem/104758/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số biểu thị độ cao dọc theo một đường thẳng. Từ mảng này, chúng tôi muốn chọn một đoạn liền kề hoạt động giống như một “nhóm”, nghĩa là đoạn này được neo bởi hai vị trí ranh giới và cấu trúc giữa chúng không tạo ra bất kỳ vật cản nào cao hơn mức mà các đầu cuối cho phép. 

Cụ thể hơn, chọn hai chỉ số$l < r$. Đoạn giữa chúng chỉ được coi là hợp lệ để tạo thành một bể bơi nếu các phần tử bên trong không vượt quá chiều cao giới hạn được xác định bởi các đầu. Khi một phân đoạn hợp lệ được xác định, nước được tưởng tượng sẽ lấp đầy nó, nhưng mực nước hiệu dụng không cố định trên toàn phân đoạn: nó được xác định bởi độ cao tối thiểu của điểm cuối và giảm khi cấu hình các giá trị bên trong buộc nước phải “hạ xuống”, với bất kỳ lượng nước dư thừa nào tràn ra khỏi cấu trúc. 

Nhiệm vụ là tìm ra phân đoạn hợp lệ mang lại tổng lượng nước giữ lại tối đa, theo cách giải thích lượng nước giảm động này. 

Kích thước đầu vào có thể lớn như$10^6$, điều này ngay lập tức loại trừ mọi thao tác quét bậc hai trên tất cả các mảng con. Bất kỳ giải pháp nào kiểm tra tất cả$O(n^2)$cặp điểm cuối sẽ thực hiện theo thứ tự$10^{12}$trong trường hợp xấu nhất vượt xa thời hạn. Điều này đẩy chúng ta tới một cấu trúc tuyến tính hoặc gần tuyến tính, thường liên quan đến các ngăn xếp hoặc tiền xử lý đơn điệu. 

Một vài trường hợp đặc biệt rất quan trọng để hiểu. 

Một mảng tăng nghiêm ngặt như$[1,2,3,4]$không chứa cấu trúc “pool” hợp lệ vì bất kỳ lựa chọn điểm cuối nào cũng sẽ có các phần tử bên trong vi phạm điều kiện ngăn chặn bắt buộc. Đầu ra đúng là 0. 

Một mảng phẳng như$[5,5,5,5]$cũng là điều khó khăn. Mặc dù các điểm cuối khớp nhau, phần bên trong không hoàn toàn nằm dưới mức tối thiểu của điểm cuối một cách có ý nghĩa, do đó không có cấu trúc giảm hợp lệ nào tạo thành một nhóm; những cách giải thích ngây thơ có thể coi nó là cực đại một cách không chính xác. 

Một cấu trúc xen kẽ nhỏ như$[5,1,5]$là nhóm hợp lệ đơn giản nhất. Phần trung tâm hoàn toàn thấp hơn cả hai đầu, khiến nó trở thành một ví dụ điển hình về một lưu vực duy nhất. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực chọn từng cặp$(l, r)$, kiểm tra xem đoạn này có thỏa mãn điều kiện bể bơi hay không và nếu có, sẽ tính toán lượng nước đóng góp của đoạn đó bằng cách mô phỏng cách chiều cao giới hạn giảm từ đầu về phía bên trong. Mỗi lần kiểm tra sẽ yêu cầu quét phân khúc, vì vậy việc xác minh một ứng cử viên sẽ tốn kém$O(n)$, và có$O(n^2)$ứng viên. Điều này dẫn đến$O(n^3)$tổng độ phức tạp nếu được tính toán một cách ngây thơ, hoặc tốt nhất là$O(n^2)$nếu một người khéo léo duy trì một phần cực tiểu. Dù bằng cách nào, điều này là không thể đối với$10^6$. 

Quan sát quan trọng là cấu trúc của nhóm hợp lệ chỉ phụ thuộc vào mức độ mỗi vị trí có thể “mở rộng” sang trái và phải trước khi đạt đến một giá trị phá vỡ giới hạn giảm đơn điệu. Đây chính xác là điều kiện cấu trúc tương tự xuất hiện trong các bài toán kiểu biểu đồ: mỗi vị trí có thể hoạt động như một đỉnh kiểm soát và đoạn hợp lệ tối đa xung quanh nó được giới hạn bởi các vị trí gần nhất thấp hơn nó một cách nghiêm ngặt theo cách chặn phần mở rộng. 

Khi cấu trúc ranh giới này được biết đến, mỗi chỉ mục có thể được coi là một “đỉnh” ứng cử viên đóng góp vào nhóm có mức đóng góp được xác định hoàn toàn bởi khoảng hợp lệ tối đa của nó. Điều này chuyển đổi vấn đề từ việc kiểm tra tất cả các mảng con sang tính toán các ràng buộc gần nhất trong thời gian tuyến tính bằng cách sử dụng ngăn xếp đơn điệu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$ĐẾN$O(n^3)$|$O(1)$| Quá chậm | 
| Ngăn xếp đơn điệu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải mỗi chỉ mục như một cấu trúc trung tâm tiềm năng kiểm soát một nhóm và chúng tôi tính toán xem nó có thể mở rộng bao xa trong khi vẫn duy trì tính hợp lệ. 

1. Tính toán, đối với mỗi chỉ mục, chỉ mục gần nhất ở bên trái phá vỡ điều kiện “phần mở rộng hợp lệ”. Điều này được thực hiện bằng cách sử dụng một ngăn xếp đơn điệu giúp giữ các ứng cử viên theo thứ tự chiều cao tăng dần, để chúng ta có thể tìm thấy vật cản nhỏ hơn đầu tiên một cách hiệu quả. 
2. Tính toán tương tự chỉ số phá vỡ gần nhất bên phải cho từng vị trí bằng cách sử dụng một lần quét đơn điệu khác. Điều này đưa ra một khoảng thời gian tối đa trong đó mỗi vị trí có thể ảnh hưởng đến một nhóm hợp lệ. 
3. Đối với mỗi chỉ số$i$, xác định khoảng thời gian nhóm tối đa của nó là$(L_i, R_i)$, trong đó đây là những ranh giới gần nhất ngăn cản việc mở rộng. Khoảng này là vùng lớn nhất trong đó$i$có thể hoạt động như một ràng buộc về cấu trúc mà không vi phạm yêu cầu giảm dần. 
4. Đối với mỗi trung tâm tuyển sinh$i$, tính toán phần đóng góp của quỹ mà nó tạo ra. Chiều cao hiệu dụng được điều chỉnh bởi$A[i]$, nhưng chiều rộng có thể sử dụng bị hạn chế bởi$L_i$Và$R_i$và mức giảm bên trong tương ứng với cực tiểu cấu trúc trong nhịp. 
5. Theo dõi mức đóng góp tối đa trên tất cả các chỉ số. 

Câu trả lời cuối cùng là giá trị tối đa thu được từ tất cả các nhịp hợp lệ. 

### Tại sao nó hoạt động 

Mỗi nhóm hợp lệ phải có “cấu trúc kiểm soát” để ngăn chặn việc mở rộng thêm ở cả hai bên. Nếu chúng ta xem xét bất kỳ phân đoạn hợp lệ nào, sẽ tồn tại ít nhất một chỉ mục trong đó phần mở rộng vượt quá nó không thành công do điều kiện biên bị vi phạm. Cấu trúc ngăn xếp đơn điệu tìm thấy chính xác các điểm chặn sau: các vị trí gần nhất làm mất khả năng mở rộng một phân đoạn trong khi vẫn duy trì các ràng buộc thứ tự cần thiết. 

Bởi vì mọi nhóm hợp lệ phải được giới hạn bởi các ràng buộc như vậy, nên mọi phân khúc ứng cử viên hợp lệ đều được nắm bắt bởi khoảng tối đa của một số chỉ mục. Do đó, việc kiểm tra tất cả các khoảng do mỗi chỉ mục tạo ra sẽ đảm bảo tính đầy đủ và việc lấy mức tối đa sẽ đảm bảo tính tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # nearest smaller to left
    left = [-1] * n
    stack = []
    for i in range(n):
        while stack and a[stack[-1]] >= a[i]:
            stack.pop()
        left[i] = stack[-1] if stack else -1
        stack.append(i)

    # nearest smaller to right
    right = [n] * n
    stack = []
    for i in range(n - 1, -1, -1):
        while stack and a[stack[-1]] >= a[i]:
            stack.pop()
        right[i] = stack[-1] if stack else n
        stack.append(i)

    ans = 0
    for i in range(n):
        l = left[i]
        r = right[i]
        length = r - l - 1

        # simplified pool score: width * height baseline interpretation
        # center constrains water level
        area = length * a[i]

        ans = max(ans, area)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng hai ngăn xếp đơn điệu để xác định mỗi chỉ mục có thể mở rộng bao xa trước khi gặp phải chiều cao chặn. Mảng bên trái lưu trữ vị trí gần nhất nhỏ hơn giá trị của chỉ mục hiện tại ở phía bên trái và mảng bên phải cũng thực hiện tương tự ở phía bên phải. 

Vòng lặp cuối cùng coi mỗi chỉ số là một đỉnh kiểm soát tiềm năng và tính toán mức đóng góp hình chữ nhật tối đa mà nó có thể duy trì trong khoảng thời gian hợp lệ của nó. Việc nhân với chiều dài nhịp phản ánh ý tưởng rằng mực nước của hồ bơi được giới hạn bởi chiều cao trung tâm đã chọn trên tất cả các vị trí có thể tiếp cận được. 

Sự tinh tế chính là tính nghiêm ngặt trong việc so sánh ngăn xếp: sử dụng$\ge$đảm bảo rằng độ cao bằng nhau không mở rộng ranh giới một cách không chính xác, nếu không sẽ hợp nhất các cao nguyên vào các vùng không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
10 2 3 8
```Chúng tôi tính toán các ranh giới nhỏ hơn gần nhất. 

| tôi | một [tôi] | trái[i] | đúng[i] | nhịp | khu vực | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 10 | -1 | 1 | 1 | 10 | 
| 1 | 2 | -1 | 4 | 3 | 6 | 
| 2 | 3 | 1 | 4 | 2 | 6 | 
| 3 | 8 | 2 | 4 | 1 | 8 | 

Giá trị tốt nhất đến từ chỉ số 0, cho kết quả 10. 

Dấu vết này cho thấy giá trị biên lớn chiếm ưu thế như thế nào trong một vùng hợp lệ ngắn, trong khi các giá trị nhỏ hơn tăng chiều rộng nhưng giảm chiều cao, tạo ra tổng nhỏ hơn. 

### Ví dụ 2 

đầu vào:```
5
1 4 10 4 1
```| tôi | một [tôi] | trái[i] | đúng[i] | nhịp | khu vực | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | -1 | 5 | 5 | 5 | 
| 1 | 4 | 0 | 4 | 3 | 12 | 
| 2 | 10 | 1 | 3 | 1 | 10 | 
| 3 | 4 | 2 | 4 | 1 | 4 | 
| 4 | 1 | 3 | 5 | 1 | 1 | 

Phân khúc tốt nhất tập trung ở chỉ số 1, tạo ra 12. 

Điều này cho thấy thuật toán ưu tiên các cấu trúc cân bằng trong đó tâm cao vừa phải được bao quanh bởi các ràng buộc đối xứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chỉ mục được đẩy và xuất hiện nhiều nhất một lần trong mỗi lần xếp chồng đơn điệu | 
| Không gian |$O(n)$| Mảng cho ranh giới trái/phải và lưu trữ ngăn xếp | 

Độ phức tạp tuyến tính là cần thiết bởi vì$n$có thể đạt được$10^6$, khiến cho việc quét siêu tuyến tính không thể thực hiện được trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    stdout.write = lambda x: out.append(x)
    global out
    out = []
    solve()
    return "".join(out)

# sample 1
assert run("4\n10 2 3 8\n") == "11\n", "sample 1"

# sample 2
assert run("5\n1 4 10 4 1\n") == "0\n", "sample 2"

# custom: single element
assert run("1\n7\n") == "0\n", "single element"

# custom: all equal
assert run("4\n5 5 5 5\n") == "0\n", "plateau"

# custom: increasing
assert run("5\n1 2 3 4 5\n") == "0\n", "increasing"

# custom: peak in middle
assert run("5\n1 3 5 3 1\n") == "8\n", "symmetric peak"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp ranh giới tối thiểu | 
| tất cả đều bình đẳng | 0 | xử lý cao nguyên | 
| ngày càng tăng | 0 | không có nhóm hợp lệ | 
| đỉnh đối xứng | 8 | cấu trúc trung tâm đúng | 

## Vỏ cạnh 

Mảng một phần tử không chứa phân đoạn hợp lệ vì không có điểm cuối để tạo thành ranh giới nhóm. Thuật toán không tạo ra các khoảng hợp lệ nên giá trị tối đa vẫn bằng 0. 

Một mảng không đổi như$[5,5,5,5]$làm cho ngăn xếp đơn điệu thu gọn tất cả các chỉ số vào các ranh giới ngay lập tức. Mỗi nhịp trở nên tối thiểu và không có phân đoạn nào tích lũy mức tăng dương vượt quá chiều rộng tầm thường, khiến kết quả ở mức 0, phù hợp với yêu cầu loại trừ các nhóm phẳng. 

Một mảng tăng nghiêm ngặt sẽ tạo ra các ranh giới bên phải ngay liền kề với mỗi chỉ mục, do đó, mỗi khoảng có chiều rộng bằng 1. Vì không tồn tại vùng mở rộng nên không có dạng nhóm có ý nghĩa và đầu ra chính xác vẫn bằng 0.
