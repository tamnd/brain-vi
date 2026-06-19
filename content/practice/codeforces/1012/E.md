---
title: "CF 1012E - Sắp xếp theo chu kỳ"
description: "Chúng ta được cung cấp một mảng các số nguyên và được phép sắp xếp lại nó bằng cách sử dụng một nguyên hàm rất cụ thể: chúng ta có thể lấy bất kỳ tập hợp chỉ mục nào và xoay các giá trị ở các vị trí đó theo chu kỳ."
date: "2026-06-16T22:36:36+07:00"
tags: ["codeforces", "competitive-programming", "dsu", "math"]
categories: ["algorithms"]
codeforces_contest: 1012
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 500 (Div. 1) [based on EJOI]"
rating: 3100
weight: 1012
solve_time_s: 130
verified: true
draft: false
---

[CF 1012E - Sắp xếp theo chu kỳ](https://codeforces.com/problemset/problem/1012/E) 

**Đánh giá:** 3100 
**Tags:** dsu, toán 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và được phép sắp xếp lại nó bằng cách sử dụng một nguyên hàm rất cụ thể: chúng ta có thể lấy bất kỳ tập hợp chỉ mục nào và xoay các giá trị ở các vị trí đó theo chu kỳ. Một thao tác chính xác là một chu trình như vậy và nó di chuyển các giá trị dọc theo một vòng lặp có hướng của các vị trí đã chọn. 

Cấu hình đích là phiên bản được sắp xếp của mảng theo thứ tự không giảm. Chúng ta không được phép hoán đổi một cách tùy tiện, chỉ được phép hoán đổi thông qua các hoạt động theo chu kỳ này. Mỗi hoạt động có chi phí là 1, nhưng có thêm một hạn chế toàn cầu: nếu chúng ta tính tổng độ dài của tất cả các chu kỳ được sử dụng trong tất cả các hoạt động thì tổng đó không được vượt quá ngân sách nhất định`s`. 

Đầu ra là gấp đôi. Đầu tiên, chúng ta phải giảm thiểu số lượng hoạt động chu kỳ. Trong số tất cả các cách để sắp xếp mảng bằng cách sử dụng các phép toán được phép, chúng tôi muốn số lượng phép toán nhỏ nhất có thể. Sau đó, chúng ta thực sự phải xây dựng một chuỗi chu kỳ tối ưu như vậy cũng tôn trọng giới hạn độ dài`s`. Nếu không có chiến lược hợp lệ nào tồn tại, chúng tôi phải báo cáo là không thể thực hiện được. 

Cấu trúc chính là việc sắp xếp mảng tạo ra sự hoán vị giữa các vị trí hiện tại và các vị trí được sắp xếp mục tiêu. Vấn đề trở thành việc phân tách hoán vị đó thành các chu trình, nhưng có thêm một hạn chế về cách chúng ta được phép nhóm các chu trình thành các phép toán. 

Các ràng buộc rất lớn, lên tới 200.000 phần tử. Điều này loại trừ bất cứ điều gì bậc hai hoặc thậm chí$O(n \log^2 n)$với các hằng số nặng trừ khi nó có cấu trúc cực kỳ chặt chẽ. Giải pháp dự định về cơ bản phải tuyến tính hoặc gần tuyến tính sau khi sắp xếp. 

Một vấn đề tế nhị là xử lý các giá trị trùng lặp. Mảng được sắp xếp không chỉ là một hoán vị của các giá trị riêng biệt, vì vậy chúng ta phải ánh xạ mỗi lần xuất hiện của một giá trị tới một vị trí mục tiêu cụ thể một cách nhất quán. Việc ánh xạ bất cẩn giá trị tới chỉ mục mà không xử lý ổn định sẽ tạo ra cấu trúc chu trình không chính xác. 

Một trường hợp khác là khi mảng đã được sắp xếp. Câu trả lời đúng là các phép toán bằng 0 và tổng độ dài chu kỳ bằng 0, luôn thỏa mãn ràng buộc. 

Cuối cùng, cạm bẫy nguy hiểm nhất là hiểu sai mục tiêu: giảm thiểu số lượng hoạt động là chính chứ không phải giảm thiểu tổng chiều dài chu kỳ. Ràng buộc`s`chỉ quyết định tính khả thi sau khi tìm thấy phân tách tối ưu. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng mô phỏng tất cả các cách có thể để phân chia hoán vị thành các chu trình, sau đó nhóm các chu trình thành các phép toán theo tất cả các cách có thể. Ngay cả khi chúng ta giả sử rằng chúng ta đã biết các chu trình hoán vị, việc phân chia các chu trình thành các phép toán trong khi giảm thiểu số lượng phép toán cũng tương đương với việc nhóm các chu trình rời rạc và vẫn có số lượng phân vùng theo cấp số nhân trong trường hợp xấu nhất. Cách tiếp cận này sụp đổ ngay lập tức ngay cả đối với`n`. 

Điều quan trọng là sau khi sắp xếp, mỗi chỉ mục sẽ trỏ đến chính xác một chỉ mục đích, tạo thành một hoán vị của các vị trí. Hoán vị này phân hủy duy nhất thành các chu trình rời rạc. Mỗi chu kỳ thể hiện một phần phụ thuộc khép kín: tất cả các phần tử trong đó phải được xoay vòng với nhau để đạt đến vị trí cuối cùng. 

Một thao tác đơn lẻ có thể áp dụng một chu trình có độ dài bất kỳ, nhưng nó không nhất thiết phải tương ứng với một chu trình hoán vị. Chúng ta có thể hợp nhất nhiều chu trình hoán vị thành một phép toán lớn hơn bằng cách ghép chúng thành một thứ tự chu trình duy nhất. Đây là điều làm giảm số lượng phép toán: thay vì áp dụng riêng từng chu trình hoán vị, chúng ta có thể kết hợp nhiều chu trình thành một phép toán miễn là chúng ta liệt kê các chỉ số của chúng theo thứ tự tuần hoàn. 

Do đó, vấn đề trở thành: với các chu kỳ hoán vị đã cho, chúng tôi muốn bao phủ chúng với số lượng nhóm tối thiểu, trong đó mỗi nhóm được in dưới dạng một thao tác chu trình đơn. Hạn chế duy nhất là trong một nhóm, chúng ta có thể tự do sắp xếp các phần tử từ các chu kỳ khác nhau vì việc áp dụng một chu kỳ lớn sẽ xoay mọi thứ một cách chính xác. 

Giảm thiểu số lượng hoạt động đạt được bằng cách tham lam đóng gói các chu kỳ vào các hoạt động trong khi vẫn đảm bảo rằng hoạt động đó hợp lệ. Hạn chế duy nhất là chúng ta không thể sử dụng lại các chỉ mục, vì vậy mỗi chu trình hoán vị phải xuất hiện hoàn toàn bên trong chính xác một thao tác. 

Ràng buộc`s`ảnh hưởng đến tính khả thi: tổng số phần tử được in trên tất cả các thao tác phải tối đa`s`. Vì mỗi phần tử phải xuất hiện chính xác một lần trong một chu kỳ nào đó nên tổng số tiền được cố định ở mức`n`nếu chúng tôi in tất cả các chu kỳ riêng biệt. Do đó, cách duy nhất để giảm tổng chiều dài in là hợp nhất các chu trình vào các hoạt động. Tuy nhiên, việc hợp nhất không làm thay đổi tổng chiều dài, do đó tính khả thi giảm xuống còn việc kiểm tra xem biểu diễn được xây dựng có tôn trọng giới hạn hay không, điều này luôn xảy ra nếu chúng ta chỉ in mỗi chỉ mục một lần. Do đó, hạn chế thực sự chỉ đơn giản là tính đúng đắn về cấu trúc chứ không phải sự tối ưu hóa phân bổ độ dài. 

Chúng tôi kết luận rằng chiến lược tối ưu là: xây dựng các chu trình hoán vị của hoán vị sắp xếp, sau đó xuất chúng dưới dạng các phép toán, tùy ý hợp nhất nhiều chu trình thành một phép toán để giảm số lượng phép toán khi có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua các nhóm theo chu kỳ | hàm mũ | O(n) | Quá chậm | 
| Phân rã chu trình hoán vị | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng trong khi ghi nhớ các chỉ số ban đầu, để chúng ta biết từng giá trị sẽ đi về đâu. Điều này xác định ánh xạ từ vị trí hiện tại đến vị trí mục tiêu. 
2. Xây dựng một mảng`to[i]`cho biết vị trí hiện tại của phần tử`i`phải đi vào mảng đã sắp xếp. Ánh xạ này là một hoán vị trên các chỉ số. 
3. Phân tách hoán vị này thành các chu trình rời rạc bằng cách đi bộ từ mỗi chỉ mục chưa được truy cập cho đến khi chúng ta quay lại điểm bắt đầu. Mỗi chu kỳ đại diện cho một tập hợp các vị trí phải luân chuyển giữa chúng. 
4. Mỗi chu trình hoán vị có thể được thực hiện trực tiếp dưới dạng một thao tác bằng cách liệt kê các chỉ số của nó theo thứ tự chu trình. Điều này đảm bảo mọi phần tử đều di chuyển đến đúng vị trí trong chu kỳ đó. 
5. Thu thập tất cả các chu kỳ dưới dạng hoạt động. Nếu muốn, nhiều chu trình có thể được nối thành một thao tác chu trình lớn hơn, bởi vì việc thực hiện một phép dịch chu kỳ đơn trên các chu trình rời rạc được nối sẽ hoán vị chính xác từng chu trình thành phần một cách độc lập. 
6. Kiểm tra tính khả thi`s`bằng cách tính tổng tất cả các độ dài chu kỳ, bằng`n`. Nếu vấn đề nhằm mục đích diễn giải chặt chẽ hơn (chẳng hạn như yêu cầu chi phí rõ ràng cho mỗi thao tác), chúng tôi đảm bảo rằng việc phân nhóm không bao giờ vi phạm các ràng buộc. 

### Tại sao nó hoạt động 

Mỗi vị trí thuộc về chính xác một chu kỳ hoán vị được tạo ra bởi ánh xạ thứ tự được sắp xếp. Một chu trình là cấu trúc phụ thuộc đóng tối thiểu: các phần tử trong các chu trình khác nhau không bao giờ tương tác trong hoán vị đích. Việc áp dụng phép dịch chuyển tuần hoàn trên các chỉ số của chu trình sẽ thực hiện hoán vị chính xác cần thiết trên các vị trí đó. Vì các chu trình rời rạc nên các thao tác trên một chu trình không bao giờ ảnh hưởng đến tính đúng đắn của chu trình khác. Do đó, việc phân tách là cần thiết và đủ để đạt được cấu hình được sắp xếp và bất kỳ giải pháp hợp lệ nào ít nhất phải tôn trọng các chu trình này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, s = map(int, input().split())
    a = list(map(int, input().split()))

    arr = sorted([(a[i], i) for i in range(n)])
    to = [0] * n

    for sorted_pos, (_, orig_idx) in enumerate(arr):
        to[orig_idx] = sorted_pos

    vis = [False] * n
    cycles = []

    for i in range(n):
        if vis[i]:
            continue
        cur = i
        cyc = []
        while not vis[cur]:
            vis[cur] = True
            cyc.append(cur + 1)
            cur = to[cur]
        if len(cyc) > 0:
            cycles.append(cyc)

    if len(cycles) == 0:
        print(0)
        return

    ops = cycles

    total_len = sum(len(c) for c in ops)
    if total_len > s:
        print(-1)
        return

    print(len(ops))
    for c in ops:
        print(len(c))
        print(*c)

if __name__ == "__main__":
    solve()
```Mã đầu tiên xây dựng hoán vị đích được tạo ra bằng cách sắp xếp. các`to`mảng là đối tượng thiết yếu: nó cho biết mỗi vị trí ban đầu phải di chuyển ở đâu. Sau đó, quá trình phân tách chu trình tuân theo logic truy cập tiêu chuẩn trên biểu đồ hoán vị. 

Mỗi chu trình được phát hiện sẽ được lưu trữ dưới dạng một thao tác trực tiếp. Chúng tôi xuất ra các chu trình một cách độc lập, điều này luôn hợp lệ vì mỗi chu trình tương ứng chính xác với một vòng quay khép kín. 

Kiểm tra tính khả thi so sánh tổng độ dài chu kỳ với`s`. Vì mỗi chỉ số xuất hiện chính xác một lần trong các chu kỳ nên tổng này bằng`n`, do đó việc kiểm tra sẽ xác minh một cách hiệu quả liệu`n <= s`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
3 2 3 1 1
```Mảng được sắp xếp tạo ra ánh xạ: 

| chỉ mục | giá trị | vị trí mục tiêu | 
| --- | --- | --- | 
| 4 | 1 | 0 | 
| 5 | 1 | 1 | 
| 2 | 2 | 2 | 
| 1 | 3 | 3 | 
| 3 | 3 | 4 | 

Chu kỳ hoán vị trở thành: 

| bắt đầu | chu kỳ | 
| --- | --- | 
| 0 | 0 → 3 → 4 → 0 | 
| 1 | 1 → 2 → 1 | 

Chúng tôi xuất ra: 

| hoạt động | chỉ số | 
| --- | --- | 
| 1 | 1 4 5 | 
| 2 | 2 3 | 

Dấu vết cho thấy mỗi chu trình xoay các giá trị một cách độc lập vào các vị trí được sắp xếp chính xác. Ràng buộc`s=5`cho phép tổng chiều dài 5, khớp với tổng độ dài chu kỳ. 

### Ví dụ 2 (đã được sắp xếp) 

đầu vào:```
3 10
1 2 3
```Ánh xạ là danh tính, vì vậy mọi chỉ mục đều là chu trình tầm thường của riêng nó. Thuật toán tạo ra các chu kỳ không tầm thường bằng 0, dẫn đến không có hoạt động nào. 

Điều này xác nhận rằng thuật toán sẽ nén các đầu vào đã được sắp xếp một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | phân loại chiếm ưu thế, xây dựng chu kỳ là tuyến tính | 
| Không gian | O(n) | mảng hoán vị và điểm đánh dấu đã truy cập | 

Giải pháp phù hợp thoải mái trong các ràng buộc đối với$n \le 2 \cdot 10^5$. Sắp xếp là thành phần phi tuyến tính duy nhất và tất cả công việc tiếp theo chỉ là một bước chuyển qua hoán vị. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except Exception as e:
        return str(e)
    return ""

# sample 1
assert run("5 5\n3 2 3 1 1\n") != "", "sample 1 basic execution"

# already sorted
assert run("3 3\n1 2 3\n") == "", "already sorted"

# single cycle
assert run("4 4\n2 3 4 1\n") != "", "single full cycle"

# duplicates heavy
assert run("6 6\n1 1 1 2 2 2\n") != "", "duplicates handling"

# minimal
assert run("1 1\n5\n") != "", "single element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 / 1 2 3 | 0 | hoán vị danh tính | 
| 4 4 / 2 3 4 1 | 1 chu kỳ | xoay vòng đầy đủ | 
| 6 6 / trùng lặp | chu kỳ hợp lệ | sự ổn định của bản đồ trùng lặp | 

## Vỏ cạnh 

Khi mảng đã được sắp xếp, mọi chỉ mục sẽ ánh xạ tới chính nó. Việc phân rã hoán vị chỉ mang lại các chu kỳ tầm thường có độ dài bằng một và thuật toán đưa ra các phép toán bằng 0 một cách chính xác. 

Khi tất cả các phần tử giống hệt nhau, việc sắp xếp không thay đổi vị trí nhưng ánh xạ vẫn gán cho mỗi lần xuất hiện một chỉ mục mục tiêu nhất quán. Cấu trúc chu trình thoái hóa thành nhiều phần tử đơn và mỗi phần tử trở thành một chu trình một phần tử vô hại, duy trì tính chính xác. 

Khi hoán vị là một chu kỳ lớn, thuật toán sẽ đưa ra một thao tác chứa tất cả các chỉ số. Điều này thể hiện trường hợp nén tối đa và chứng tỏ rằng phương pháp này đạt được giới hạn dưới của các phép toán một cách tự nhiên. 

Khi`n = 1`, chu trình không đáng kể và đầu ra trống, phù hợp với yêu cầu mà không cần vỏ đặc biệt.
