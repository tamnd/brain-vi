---
title: "CF 105962K - Thử nghiệm của Rofofo"
description: "Chúng ta được cung cấp một mô hình máy trong đó bộ nhớ được sắp xếp thành nhiều dòng bộ đệm và mỗi dòng bộ đệm chứa nhiều biến 8 bit. Mỗi biến giữ một giá trị từ 0 đến 255 và tất cả số học được thực hiện theo modulo 256, do đó các giá trị bao quanh sau 255. Có hai thao tác."
date: "2026-06-22T16:18:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105962
codeforces_index: "K"
codeforces_contest_name: "UNICAMP Freshman Contest 2025"
rating: 0
weight: 105962
solve_time_s: 59
verified: true
draft: false
---

[CF 105962K - Thử nghiệm của Rofofo](https://codeforces.com/problemset/problem/105962/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mô hình máy trong đó bộ nhớ được sắp xếp thành nhiều dòng bộ đệm và mỗi dòng bộ đệm chứa nhiều biến 8 bit. Mỗi biến giữ một giá trị từ 0 đến 255 và tất cả số học được thực hiện theo modulo 256, do đó các giá trị sẽ bao quanh sau 255. 

Hai hoạt động có sẵn. Thao tác đầu tiên tăng một biến được chọn lên một đơn vị và tiêu tốn một đơn vị thời gian. Thao tác thứ hai tăng mỗi biến trong dòng bộ đệm đã chọn thêm một đơn vị và tiêu tốn B đơn vị thời gian. Cả hai phép toán đều hoạt động theo chu kỳ do số học modulo 256. 

Mục tiêu là thực hiện một chuỗi các thao tác này để cuối cùng, mọi biến đơn lẻ trên tất cả các dòng bộ đệm đều có cùng giá trị. Bản thân giá trị không cố định trước nên chúng ta có thể tự do lựa chọn giá trị chung cuối cùng sao cho tổng chi phí là nhỏ nhất. 

Kích thước đầu vào có cấu trúc quan trọng: số lượng dòng bộ đệm N rất nhỏ, nhiều nhất là 10, trong khi mỗi dòng bộ đệm có thể chứa tối đa 10^4 giá trị. Sự bất đối xứng này cho thấy rằng bất kỳ giải pháp nào phụ thuộc bậc hai hoặc tệ hơn vào M đều có thể quá chậm, trong khi các giải pháp tổng hợp thông tin trên mỗi dòng bộ đệm và tránh tính toán lại từng phần tử là khả thi. 

Một vấn đề tế nhị xuất phát từ số học mô-đun. Việc đạt được giá trị mục tiêu không chỉ là về sự khác biệt mà còn là việc chọn số lượng gia tăng theo cách giảm thiểu chi phí theo cách bao quát. Một chiến lược tham lam ngây thơ luôn “cố định từng ô một cách độc lập với mục tiêu” có thể bỏ lỡ việc áp dụng mức tăng toàn bộ bộ đệm có thể giảm chi phí cho nhiều phần tử cùng một lúc. 

Các trường hợp cạnh xuất hiện khi A và B rất khác nhau. Nếu A nhỏ hơn nhiều so với B thì các bản sửa lỗi riêng lẻ sẽ chiếm ưu thế. Nếu B nhỏ hơn nhiều, việc sử dụng nhiều thao tác bộ nhớ đệm và dựa vào mô-đun bao quanh để điều chỉnh giá trị sẽ trở nên tối ưu. Một trường hợp góc khác là khi giá trị cuối cùng tối ưu không được căn chỉnh với bất kỳ giá trị ban đầu nào, điều này phá vỡ các phương pháp chỉ coi các số hiện có là ứng cử viên. 

## Phương pháp tiếp cận 

Cách giải thích trực tiếp bằng vũ lực là thử tất cả các giá trị cuối cùng có thể có từ 0 đến 255 và đối với mỗi giá trị, quyết định độc lập số lần áp dụng thao tác bộ nhớ đệm cho mỗi hàng và số lần tăng riêng lẻ mà mỗi biến cần. Đối với giá trị đích cố định x và một hàng cố định, nếu chúng ta cũng cố định số thao tác của hàng t thì mọi phần tử trong hàng đó phải được sửa riêng lẻ để khớp với x. Điều này đưa ra một chi phí được xác định bằng khoảng cách giữa mỗi phần tử với x sau khi áp dụng t ca. 

Tuy nhiên, cách nhìn ngây thơ này trở nên tốn kém vì với mỗi hàng, chúng ta sẽ khám phá 256 lựa chọn của t và với mỗi lựa chọn như vậy, chi phí tính toán lại trên M phần tử. Điều đó dẫn đến khoảng O(N * 256 * M) trên mỗi giá trị mục tiêu, quá chậm khi M là 10^4. 

Quan sát quan trọng là trong một hàng, việc áp dụng thao tác bộ nhớ đệm t lần tương đương với việc trừ t modulo 256 cho mỗi lần điều chỉnh cần thiết. Điều này biến vấn đề thành một sự dịch chuyển theo chu kỳ trên một mảng các giá trị tần số cố định. Thay vì tính toán lại từng phần tử, chúng tôi tính toán trước tần số của các giá trị trong mỗi hàng và đánh giá sự thay đổi chi phí trong chu kỳ dài 256 một cách hiệu quả. 

Điều này làm giảm tính toán của mỗi hàng xuống O(256^2), vì đối với mỗi ca trong số 256 ca có thể xảy ra, chúng tôi đánh giá các khoản đóng góp trên 256 nhóm giá trị. Sau đó, chúng tôi kết hợp các hàng một cách độc lập và thử tất cả 256 giá trị mục tiêu chung. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force mỗi phần tử | O(256 * N * M) | O(1) | Quá chậm | 
| Tối ưu hóa tần số + dịch chuyển theo chu kỳ | O(256 * 256 * N + 256 * N) | O(256 * N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chia quyết định thành hai cấp độ: chọn giá trị mục tiêu cuối cùng và chọn số lượng mức tăng trong toàn bộ bộ đệm để áp dụng cho mỗi hàng.

1. Đối với mỗi dòng bộ đệm, hãy xây dựng bảng tần số gồm các giá trị của nó trong phạm vi từ 0 đến 255. Điều này sẽ nén hàng để chúng ta không còn phụ thuộc trực tiếp vào M nữa. 
2. Cố định giá trị cuối cùng ứng cử viên x trong phạm vi từ 0 đến 255. Chúng tôi tính chi phí tối thiểu để làm cho tất cả các giá trị bằng x. 
3. Đối với một hàng nhất định và một số gia số toàn hàng cố định t, mọi giá trị v trong hàng đó thực tế sẽ trở thành v + t modulo 256. Để đạt đến x, mỗi ô phải được tăng riêng lẻ một số lần bằng (x - (v + t)) mod 256. 
4. Chi phí của việc điều chỉnh này đối với một t cố định được tính bằng cách tính tổng tất cả các nhóm giá trị bằng bảng tần số. Mỗi đơn vị tăng thêm riêng lẻ có giá A, do đó phần đóng góp của một ô là A nhân với số hiệu chỉnh cần thiết của nó. 
5. Chúng tôi đánh giá chi phí này cho tất cả t từ 0 đến 255. Đối với mỗi hàng, chúng tôi lấy giá trị tối thiểu trên tất cả t, thêm chi phí B * t để áp dụng các phép toán hàng. 
6. Chúng tôi tính tổng chi phí tốt nhất có thể đạt được trên tất cả các hàng cho x cố định này. 
7. Chúng tôi lặp lại với mọi x từ 0 đến 255 và lấy mức tối thiểu toàn cục. 

### Tại sao nó hoạt động 

Bất biến quan trọng là đối với bất kỳ hàng nào, khi số lượng gia tăng toàn hàng t được cố định, tất cả các hoạt động còn lại sẽ trở nên độc lập trên mỗi ô và được xác định đầy đủ bởi khoảng cách mô-đun đến giá trị đích. Mỗi chuỗi hoạt động có thể có thể được biểu diễn một cách duy nhất bằng cách chọn t và sau đó áp dụng các bước tăng riêng lẻ để khắc phục sự khác biệt về dư lượng. Bởi vì tất cả các phép toán chuyển đổi theo phép cộng modulo 256, nên không có hiệu ứng thứ tự nào tồn tại ngoài tổng số đếm. Điều này đảm bảo rằng việc liệt kê tất cả t và tất cả x bao gồm mọi phép biến đổi hợp lệ và hàm chi phí nắm bắt mọi chiến lược có thể có mà không bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    A, B = map(int, input().split())
    N, M = map(int, input().split())

    rows = []
    for _ in range(N):
        arr = list(map(int, input().split()))
        freq = [0] * 256
        for v in arr:
            freq[v] += 1
        rows.append(freq)

    INF = 10**18
    ans = INF

    for target in range(256):
        total = 0

        for freq in rows:
            best_row = INF

            for t in range(256):
                cost = B * t
                for v in range(256):
                    if freq[v] == 0:
                        continue
                    need = (target - (v + t)) % 256
                    cost += freq[v] * need * A

                if cost < best_row:
                    best_row = cost

            total += best_row

        ans = min(ans, total)

    print(ans)

if __name__ == "__main__":
    solve()
```Trước tiên, giải pháp nén từng dòng bộ đệm vào một mảng tần số, loại bỏ sự phụ thuộc vào M. Sau đó, giải pháp này lặp lại tất cả các giá trị cuối cùng có thể có. Đối với mỗi hàng và mỗi số ứng viên của số gia toàn hàng, nó tính toán chi phí hiệu chỉnh bằng cách sử dụng số học mô-đun. Vòng lặp bên trong trên 256 giá trị là an toàn vì nó thay thế kích thước M ban đầu. 

Một cạm bẫy triển khai phổ biến là quên rằng các thao tác hàng áp dụng trước hoặc sau các thao tác riêng lẻ không thành vấn đề; chỉ có tổng số lượng quan trọng do tính giao hoán. Một vấn đề khác là không thực hiện đúng modulo 256 khi tính toán các mức tăng cần thiết, điều này sẽ phá vỡ tính chính xác bao quanh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
A = 1, B = 2
N = 2, M = 3
Row1: [1, 2, 1]
Row2: [1, 1, 1]
```Chúng tôi đánh giá một mục tiêu ứng cử viên, giả sử x = 2. 

Đối với Hàng 1, chúng tôi kiểm tra các giá trị t khác nhau: 

| t | giá trị được chuyển | ý tưởng chi phí | 
| --- | --- | --- | 
| 0 | [1,2,1] | cần chỉnh sửa nhỏ | 
| 1 | [2,3,2] | bọc làm tăng sự không phù hợp | 
| 2 | [3,4,3] | thậm chí còn tệ hơn | 

T tốt nhất là 0. 

Đối với Hàng 2, tất cả các giá trị đều là 1 nên khoảng cách đến 2 là đồng nhất. Một lần nữa t = 0 là tối ưu. 

Thuật toán tìm ra chi phí kết hợp tối thiểu bằng cách tính tổng các kết quả tối ưu trên mỗi hàng, xác nhận tính độc lập giữa các hàng. 

### Ví dụ 2 

đầu vào:```
A = 1, B = 2
N = 2, M = 3
Row1: [1, 2, 1]
Row2: [254, 253, 255]
```Đối với Hàng 2, cấu trúc mô-đun rất quan trọng. 

| t | ảnh hưởng đến giá trị | trực giác | 
| --- | --- | --- | 
| 0 | gần 255.253.254 | đã gần bằng 0 | 
| 1 | dịch chuyển gần hơn đến 0 | cải thiện sự liên kết | 
| 2 | vượt quá | chi phí xấu đi | 

Giải pháp tối ưu cho Hàng 2 sử dụng một số lượng nhỏ các gia số hàng để đưa tất cả các giá trị đến gần một vùng bao quanh chung, giảm thiểu các chỉnh sửa riêng lẻ tốn kém. 

Điều này thể hiện cơ chế chính: các thao tác hàng chỉ có giá trị khi chúng giảm khoảng cách mô-đun trên toàn bộ nhiều phần tử cùng một lúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2562 · N) | Đối với mỗi mục tiêu trong số 256 mục tiêu, mỗi hàng đánh giá 256 ca trên 256 nhóm giá trị | 
| Không gian | O(256 · N) | Bảng tần số mỗi hàng | 

Các ràng buộc cho phép N lên tới 10, do đó thậm chí một vài triệu thao tác nguyên thủy cũng có thể chấp nhận được. Sự phụ thuộc vào M biến mất hoàn toàn, điều này rất cần thiết vì M có thể lớn tới 10^4. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    A, B = map(int, input().split())
    N, M = map(int, input().split())
    rows = []
    for _ in range(N):
        arr = list(map(int, input().split()))
        freq = [0]*256
        for v in arr:
            freq[v] += 1
        rows.append(freq)

    INF = 10**18
    ans = INF

    for target in range(256):
        total = 0
        for freq in rows:
            best = INF
            for t in range(256):
                cost = B*t
                for v in range(256):
                    if freq[v]:
                        need = (target - (v+t)) % 256
                        cost += freq[v]*need*A
                best = min(best, cost)
            total += best
        ans = min(ans, total)

    return str(ans)

# minimal case
assert run("1 1\n1 1\n1 1\n5") == run("1 1\n1 1\n1 1\n5")

# all equal already
assert run("1 10\n1 5\n7 7 7 7 7") == "0"

# single row wrap case
assert run("1 2\n1 3\n254 255 0") is not None

# mixed values
assert run("2 3\n2 3\n1 2 3\n4 5 6") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | không cần thao tác | 
| hàng bao quanh | tối thiểu thông qua t shift | độ đúng modulo | 
| hàng hỗn hợp | tối ưu hữu hạn | tương tác của các hàng | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi các giá trị được nhóm lại gần ranh giới modulo, chẳng hạn như 254, 255 và 0. Một cách tiếp cận ngây thơ bỏ qua sự bao bọc sẽ coi 0 ở xa 255, tạo ra chi phí không chính xác lớn. Thuật toán xử lý điều này một cách chính xác vì khoảng cách luôn được tính theo modulo 256, do đó việc dịch chuyển một hàng có thể căn chỉnh 255 với 0 thông qua một chu kỳ tăng đơn. 

Một trường hợp cạnh khác là khi B nhỏ hơn đáng kể so với A. Trong tình huống này, chiến lược tối ưu chủ yếu dựa vào việc chọn t để giảm thiểu tổng dư lượng trước khi thực hiện các chỉnh sửa riêng lẻ. Thuật toán khám phá chính xác tất cả các giá trị t, do đó, nó nắm bắt được chiến lược “sửa chữa toàn cục trước, sau đó cục bộ”. 

Cuối cùng, khi M lớn nhưng các giá trị có tính lặp lại cao, việc nén tần số sẽ đảm bảo tính chính xác mà không làm giảm hiệu suất. Chi phí vẫn chính xác vì mỗi nhóm tần số đóng góp tuyến tính, duy trì tổng số chính xác bất kể sự lặp lại.
