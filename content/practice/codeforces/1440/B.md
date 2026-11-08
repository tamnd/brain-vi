---
title: "CF 1440B - Tổng số trung vị"
description: "Chúng ta được cung cấp một danh sách được sắp xếp gồm các số $nk$ và được yêu cầu chia nó thành các nhóm $k$, mỗi nhóm chứa chính xác các phần tử $n$."
date: "2026-06-11T04:23:39+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1440
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 684 (Div. 2)"
rating: 900
weight: 1440
solve_time_s: 71
verified: true
draft: false
---

[CF 1440B - Tổng số trung vị](https://codeforces.com/problemset/problem/1440/B) 

**Xếp hạng:** 900 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách được sắp xếp$nk$số và yêu cầu chia nó thành$k$nhóm, mỗi nhóm chứa chính xác$n$các phần tử. Sau khi phân chia xong, chúng tôi tính toán trung vị của mỗi nhóm bằng cách sử dụng vị trí ở giữa phía trên theo thứ tự được sắp xếp của nó và chúng tôi muốn tổng của tất cả các trung vị này càng lớn càng tốt. 

Hạn chế chính là chúng ta không thể tự do sắp xếp lại mảng đầu vào một cách tùy ý trong các nhóm mà không mất phí, nhưng mảng toàn cục đã được sắp xếp. Nhiệm vụ hoàn toàn là làm thế nào để phân chia các giá trị liên tiếp thành các nhóm sao cho các vị trí trung vị được chọn trở nên lớn nhất có thể. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số phần tử trên tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng tất cả các phân vùng có thể có hoặc sắp xếp các tập hợp con nhiều lần. Bất kỳ cách tiếp cận nào về cơ bản đều phải tuyến tính về kích thước mảng cho mỗi trường hợp thử nghiệm hoặc tệ nhất là$O(nk \log(nk))$, mặc dù việc sắp xếp là không cần thiết vì mảng đã được sắp xếp rồi. 

Trường hợp cạnh tinh tế xuất hiện khi$n = 1$. Trong tình huống đó, mỗi phần tử đều có trung vị riêng của nó, vì vậy câu trả lời chỉ là tổng của mảng. Một chiến lược nhóm ngây thơ cố gắng “cân bằng” các nhóm có thể dễ dàng bỏ lỡ sự đơn giản hóa này. 

Một trường hợp góc không rõ ràng khác xảy ra khi$k = 1$. Khi đó chúng ta chỉ có một nhóm, vì vậy câu trả lời đơn giản là giá trị trung bình của toàn bộ mảng, tức là phần tử ở chỉ mục$\lceil nk/2 \rceil$. 

Khó khăn chính nảy sinh khi cả hai$n$Và$k$lớn, bởi vì chúng ta phải quyết định cách phân phối các phần tử sao cho số trung vị có được từ các giá trị sẵn có lớn nhất có thể. 

## Phương pháp tiếp cận 

Nếu chúng ta nghĩ về bạo lực, chúng ta sẽ thử mọi cách có thể để chia mảng đã sắp xếp thành$k$nhóm kích thước$n$. Ngay cả khi chúng ta hạn chế duy trì trật tự, số cách chọn vị trí là tổ hợp, theo thứ tự các hệ số đa thức. Vì$nk \le 2000$, điều này đã không thể thực hiện được rồi; vì$2 \cdot 10^5$, điều đó là hoàn toàn không thể. 

Quan sát quan trọng là mảng được sắp xếp và chúng tôi chỉ quan tâm đến số trung vị chứ không quan tâm đến cấu trúc nhóm đầy đủ. Bên trong mỗi nhóm kích thước$n$, trung vị là phần tử ở vị trí$\lceil n/2 \rceil$. Vì vậy mỗi nhóm đóng góp một yếu tố “quan trọng”, còn lại chỉ là những ràng buộc đảm bảo tính khả thi. 

Điều này cho thấy chúng ta nên suy nghĩ về việc lựa chọn$k$vị trí trung bình từ mảng. Cấu trúc trở nên rõ ràng hơn nếu chúng ta tưởng tượng việc xây dựng các nhóm một cách tham lam từ các giá trị lớn nhất ngược trở lại: để tối đa hóa trung vị, chúng ta muốn mỗi trung vị càng lớn càng tốt, nhưng chúng ta buộc phải “tiêu tốn” các phần tử khác xung quanh nó để hoàn thành mỗi nhóm. 

Mỗi trung vị tiêu thụ một cách hiệu quả$n - 1$các yếu tố hỗ trợ. Vì muốn tối đa hóa số trung vị, chúng tôi nên chỉ định các phần tử có sẵn lớn nhất làm số trung vị, nhưng chúng tôi phải đảm bảo vẫn còn đủ phần tử để tạo thành các nhóm hợp lệ. 

Nếu chúng tôi xử lý từ các giá trị lớn nhất, mỗi khi chọn trung vị, chúng tôi phải bỏ qua đủ phần tử để có thể hoàn thành nhóm. Mẫu tối ưu hóa ra có tính xác định: chúng ta liên tục lấy một phần tử làm trung vị, sau đó loại bỏ$n - 1$các yếu tố có thể cản trở việc hình thành các nhóm hợp lệ. 

Tương tự, chúng ta có thể làm việc từ cuối mảng và chọn mọi phần tử thứ hai bắt đầu từ một phần bù cụ thể được xác định bởi$n$. Khoảng cách giữa các trung vị được chọn chính xác$n - 1$, nhưng vì các nhóm trùng nhau trong cấu trúc được sắp xếp nên bước hiệu quả sẽ trở thành$n - 1$các phần tử bị bỏ qua giữa các dải phân cách được chọn liên tiếp khi quét từ phải sang trái. 

Như vậy, giải pháp tối ưu rút gọn lại việc lựa chọn$k$các phần tử từ mảng được sắp xếp theo các khoảng cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các phân vùng) | hàm mũ | cao | Quá chậm | 
| Lựa chọn tham lam tối ưu |$O(nk)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán$m = \lceil n/2 \rceil$, đó là vị trí trung bình trong bất kỳ nhóm nào. Điều này cho chúng ta biết khoảng cách giữa trung vị tính từ điểm bắt đầu của nhóm khi được sắp xếp. 
2. Làm việc trên mảng được sắp xếp toàn cục từ đầu, vì các giá trị lớn hơn sẽ đóng góp vào giá trị trung vị bất cứ khi nào có thể. 
3. Bắt đầu từ chỉ mục$nk - 1$, chọn các phần tử làm trung vị trong khi di chuyển sang trái theo các bước có kích thước$n - m + 1$hoặc đơn giản hơn là bằng mẫu bắt nguồn từ các ràng buộc nhóm: mỗi khi chúng ta chọn một trung vị, chúng ta bỏ qua$n - m$các phần tử phải thuộc cùng một nhóm hoặc nhóm trước đó. 

Việc triển khai sạch sẽ đơn giản hóa việc này hơn nữa: chúng tôi lùi lại và chọn mọi$(n - m + 1)$-phần tử thứ bắt đầu từ một offset cụ thể. 
4. Thu thập chính xác$k$những phần tử như vậy, vì có$k$các nhóm và do đó$k$trung vị. 
5. Tính tổng các phần tử đã chọn này và đưa ra kết quả. 

Phần tinh tế nhất là chúng ta không xây dựng nhóm một cách rõ ràng. Thay vào đó, chúng tôi mô phỏng các ràng buộc do nhóm áp đặt: mỗi trung vị phải có đủ phần tử ở bên phải (theo thứ tự được sắp xếp) để điền vào nhóm của nó. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi chọn được số trung vị, chúng ta luôn để lại đủ các phần tử chưa sử dụng để tạo thành nhóm hoàn chỉnh cho tất cả các số trung vị còn lại. Bởi vì mảng được sắp xếp, việc lấy trung vị từ bên phải sẽ tối đa hóa giá trị của chúng và bỏ qua chính xác số phần tử cần thiết để đảm bảo tính khả thi mà không lãng phí các phần tử lớn hơn ở các vị trí không phải trung vị. Bất kỳ sai lệch nào chọn trung vị nhỏ hơn trước đó sẽ trực tiếp làm giảm tổng hoặc buộc các phần tử lớn hơn nữa vào các vị trí không phải trung vị, điều này không thể làm tăng tổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        # median position in each group (1-indexed)
        m = (n + 1) // 2
        
        # step size between useful medians when scanning from right
        step = n - m + 1
        
        # start from the last element
        idx = len(a) - step
        
        ans = 0
        
        for _ in range(k):
            ans += a[idx]
            idx -= step
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai hoạt động hoàn toàn dựa trên số học chỉ mục trên mảng được sắp xếp. Điều quan trọng là tính toán khoảng cách chính xác giữa các dải phân cách đã chọn. Biến`m`xác định vị trí trung vị nằm trong mỗi nhóm và từ đó chúng ta rút ra bao nhiêu phần tử phải bỏ qua để mỗi trung vị được chọn có thể thuộc về một nhóm hợp lệ mà không bị trùng lặp. 

Việc duyệt ngược đảm bảo chúng ta luôn chọn được ứng cử viên hợp lệ lớn nhất hiện có. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2, k = 4
a = [0, 24, 34, 58, 62, 64, 69, 78]
```Đây$m = 1$, vì vậy mọi phần tử đầu tiên trong nhóm có kích thước 2 đều là số trung vị sau khi sắp xếp từng cặp. 

Chúng tôi tính toán`step = 2`. 

| Bước | idx | Giá trị được chọn | Còn lại k | 
| --- | --- | --- | --- | 
| 1 | 6 | 69 | 3 | 
| 2 | 4 | 62 | 2 | 
| 3 | 2 | 34 | 1 | 
| 4 | 0 | 0 | 0 | 

Tổng = 165. 

Điều này xác nhận rằng chúng tôi luôn lấy giá trị trung bình hợp lệ lớn nhất có thể trong khi vẫn tôn trọng các ràng buộc về khoảng cách. 

### Ví dụ 2 

đầu vào:```
n = 3, k = 3
a = [2, 4, 16, 18, 21, 27, 36, 53, 82]
```Đây$m = 2$, vì vậy trung vị là phần tử thứ hai trong mỗi nhóm và`step = 2`. 

| Bước | idx | Giá trị được chọn | Còn lại k | 
| --- | --- | --- | --- | 
| 1 | 7 | 53 | 2 | 
| 2 | 5 | 27 | 1 | 
| 3 | 3 | 18 | 0 | 

Tổng = 98. 

Mỗi lựa chọn đảm bảo vẫn còn đủ yếu tố để tạo thành các nhóm hợp lệ và chúng tôi luôn ưu tiên mức trung bình khả thi lớn nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nk)$| Mỗi phần tử được xử lý một lần trong lần quét tuyến tính cho mỗi trường hợp thử nghiệm | 
| Không gian |$O(1)$| Chỉ số học chỉ số và bộ tích lũy được sử dụng | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử, do đó việc truyền tuyến tính qua mảng dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    
    input = sys.stdin.readline
    t = int(sys.stdin.readline())
    
    out = []
    for _ in range(t):
        n, k = map(int, sys.stdin.readline().split())
        a = list(map(int, sys.stdin.readline().split()))
        
        m = (n + 1) // 2
        step = n - m + 1
        
        idx = len(a) - step
        ans = 0
        
        for _ in range(k):
            ans += a[idx]
            idx -= step
        
        out.append(str(ans))
    
    return "\n".join(out)

# provided samples (abbreviated check)
assert run("""6
2 4
0 24 34 58 62 64 69 78
2 2
27 61 81 91
4 3
2 4 16 18 21 27 36 53 82 91 92 95
3 4
3 11 12 22 33 35 38 67 69 71 94 99
2 1
11 41
3 3
1 1 1 1 1 1 1 1 1
""") == """165
108
145
234
11
3"""

# custom cases
assert run("""1
1 5
5 4 3 2 1
""") == "15"

assert run("""1
2 1
100 200
""") == "100"

assert run("""1
3 2
1 2 3 4 5 6
""") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 trường hợp | tổng hợp | hành vi trung bình tầm thường | 
| k=1 trường hợp | trung vị đơn | độ chính xác trung bình đầy đủ của mảng | 
| trường hợp hỗn hợp | 8 | khoảng cách đúng đắn | 

## Vỏ cạnh 

Khi nào$n = 1$, mỗi phần tử là nhóm riêng của nó và cũng là trung vị của chính nó. Thuật toán giảm xuống việc chọn mọi phần tử ở bước 1. Số học chỉ mục vẫn hoạt động vì$m = 1$cho`step = n - m + 1 = 1`, vì vậy chúng tôi chỉ cần tính tổng toàn bộ mảng. 

Khi$k = 1$, chúng tôi chỉ chọn một phần tử. Chỉ số bắt đầu trở thành vị trí trung bình chính xác của toàn bộ mảng, vì`step`căn chỉnh chỉ mục được chọn đầu tiên với vị trí trung bình toàn cầu. Thuật toán trả về phần tử trung tâm của mảng được sắp xếp một cách hiệu quả. 

Khi tất cả các giá trị đều bằng nhau, mọi nhóm đều mang lại giá trị trung vị giống hệt nhau. Thuật toán vẫn chọn các phần tử có khoảng cách đều nhau nhưng tổng vẫn nhất quán bất kể phân vùng, xác nhận tính đúng đắn khi suy biến. 

Trong trường hợp ranh giới nơi$n$lớn và$k$nhỏ, kích thước bước đảm bảo chúng ta không bao giờ tiêu thụ quá nhiều phần tử, bởi vì mỗi trung vị được chọn ngầm dự trữ đủ phần tử để tạo thành một nhóm hợp lệ xung quanh nó.
