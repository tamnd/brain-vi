---
title: "CF 1692H - Cờ bạc"
description: "Chúng ta biết trước kết quả của lần tung xúc xắc $n$ tiếp theo. Marian chọn một giá trị $a$ và một phân đoạn liền kề của các vòng $[l,r]$. Trong mỗi vòng của phân đoạn đó, anh ấy luôn đoán cùng một giá trị $a$. Bất cứ khi nào giá trị cuộn thực tế bằng $a$, số tiền của anh ta sẽ tăng gấp đôi."
date: "2026-06-09T23:06:15+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1692
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 799 (Div. 4)"
rating: 1700
weight: 1692
solve_time_s: 186
verified: false
draft: false
---

[CF 1692H - Cờ bạc](https://codeforces.com/problemset/problem/1692/H) 

**Đánh giá:** 1700 
**Tags:** cấu trúc dữ liệu, dp, tham lam, toán học 
**Thời gian giải:** 3 phút 6s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta biết kết quả tiếp theo$n$xúc xắc tung trước. Marian chọn một giá trị$a$, và một đoạn liền kề của các vòng$[l,r]$. Trong mỗi vòng ở phân đoạn đó, anh ấy luôn đoán cùng một giá trị$a$. 

Bất cứ khi nào giá trị cuộn thực tế bằng$a$, số tiền của anh ta tăng gấp đôi. Nếu không thì nó sẽ giảm đi một nửa. 

Vì Marian bắt đầu với chính xác 1 đô la nên chúng ta cần tìm giá trị$a$và phân đoạn$[l,r]$để tối đa hóa số tiền cuối cùng của anh ta. 

Quan sát đầu tiên là chỉ có hai lũy thừa xuất hiện. Nếu một đoạn chứa$k$đoán thành công và$m$đoán không thành công thì số tiền cuối cùng là$$2^k \cdot 2^{-m} = 2^{k-m}.$$Tối đa hóa số tiền cũng giống như tối đa hóa số mũ$k-m$. 

Tổng chiều dài của một đoạn là$k+m$, Vì thế$$k-m = k-(\text{length}-k)=2k-\text{length}.$$Đối với một giá trị cố định$a$, mỗi lần xuất hiện của$a$đóng góp$+1$ĐẾN$k$, trong khi mọi thứ không$a$vị trí đóng góp$-1$đến số mũ. Thách thức là tìm ra giá trị tốt nhất và phân khúc tốt nhất. 

Những hạn chế là lớn. Tổng của tất cả$n$giá trị trên các trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$. Bất kỳ giải pháp nào kiểm tra tất cả các cặp$(l,r)$là không thể bởi vì điều đó sẽ đòi hỏi khoảng$n^2$công việc. Với$n=2\cdot10^5$, các thuật toán bậc hai sẽ thực hiện hàng chục tỷ phép tính. 

Chúng ta cần một cái gì đó gần tuyến tính hoặc$O(n \log n)$. 

Trường hợp cạnh tinh tế xuất hiện khi một giá trị chỉ xuất hiện một lần. 

Ví dụ:```
1
5
1 2 3 4 5
```Câu trả lời tốt nhất là bất kỳ vị trí nào. Việc chọn một đoạn dài hơn chỉ gây ra tổn thất. Giải pháp luôn cố gắng kết nối nhiều lần xuất hiện sẽ bỏ lỡ trường hợp này. 

Một tình huống phức tạp khác xảy ra khi hai lần xuất hiện cách nhau rất xa.```
1
5
7 1 1 1 7
```Sử dụng toàn bộ phân đoạn cho giá trị 7 sẽ cho số mũ$$2\cdot2-5=-1.$$Chỉ sử dụng một lần xuất hiện sẽ cho số mũ$1$. 

Câu trả lời tối ưu là đoạn có độ dài bằng một, không phải đoạn chứa cả hai lần xuất hiện. 

Cạm bẫy thứ ba là phân đoạn tối ưu cho một giá trị không nhất thiết phải bắt đầu và kết thúc ở những lần xuất hiện liên tiếp. 

Coi như:```
1
7
5 2 5 3 4 5 5
```Phân đoạn tốt nhất cho giá trị 5 bao gồm tất cả bốn lần xuất hiện mặc dù có khoảng trống. Chúng ta cần một phương pháp tự động quyết định những lần xuất hiện nào sẽ thuộc phân đoạn tối ưu. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi giá trị có thể$a$, mọi điểm cuối bên trái và mọi điểm cuối bên phải. Với mỗi đoạn nó sẽ đếm xem có bao nhiêu vị trí bằng nhau$a$và tính số mũ kết quả. 

Điều này đúng vì nó đánh giá rõ ràng mọi chiến lược pháp lý. 

Vấn đề là thời gian chạy. có$O(n^2)$phân khúc, và đánh giá từng phân khúc một cách ngây thơ sẽ gây tốn kém cho một phân khúc khác$O(n)$. Ngay cả sau khi tần số tiền xử lý, việc kiểm tra tất cả các phân đoạn vẫn còn$O(n^2)$, vượt xa giới hạn. 

Quan sát quan trọng đến từ việc viết lại điểm số. 

Sửa một số giá trị$a$. Đặt vị trí xuất hiện của nó là$$p_1 < p_2 < \cdots < p_m.$$Giả sử một phân đoạn bắt đầu khi xuất hiện$p_i$và kết thúc khi xảy ra$p_j$. Bất kỳ phân khúc tối ưu nào cho giá trị$a$có thể được coi là bắt đầu và kết thúc khi xuất hiện$a$, bởi vì việc mở rộng ra ngoài chúng chỉ tạo thêm thất bại. 

Bên trong phân khúc này:$$k = j-i+1.$$chiều dài là$$p_j-p_i+1.$$Số mũ trở thành$$2(j-i+1) - (p_j-p_i+1).$$Mở rộng:$$(j-i+1) - (p_j-p_i).$$Sắp xếp lại:$$(p_i-i) - (p_j-j) + 1.$$Bây giờ xác định$$b_t = p_t - t.$$Để cố định$j$, tối đa hóa điểm có nghĩa là tìm giá trị lớn nhất trước đó của$b_i$. 

Điều này trở thành vấn đề về kiểu mảng con tối đa trong danh sách xuất hiện của từng giá trị riêng biệt. 

Một cách khác để xem nó thậm chí còn đơn giản hơn. Giữa hai lần xuất hiện liên tiếp có$$p_t-p_{t-1}-1$$không-$a$các phần tử. Việc kết nối những lần xuất hiện đó sẽ thay đổi điểm số bằng cách$$1-(p_t-p_{t-1}-1) = 2-(p_t-p_{t-1}).$$Điều này mang lại một chương trình động giống Kadane tự nhiên đối với các lần xuất hiện của từng giá trị. 

Vì mỗi vị trí mảng thuộc về chính xác một danh sách xuất hiện nên tổng công việc trên tất cả các giá trị là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$ĐẾN$O(n^3)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$mỗi trường hợp thử nghiệm |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi giá trị riêng biệt, hãy thu thập tất cả các vị trí mà nó xuất hiện. 

Đặt các vị trí là:$$p_1,p_2,\ldots,p_m.$$Chúng tôi chạy DP kiểu Kadane trong danh sách xuất hiện này. 

1. Bắt đầu một phân đoạn ở lần xuất hiện đầu tiên. 
2. Hãy để`cur`là điểm của phân đoạn tốt nhất kết thúc ở lần xuất hiện hiện tại. 

Một lần xuất hiện duy nhất sẽ đóng góp điểm 1 vì một lần đoán đúng và không sai sẽ mang lại số mũ 1. 
3. Khi chuyển từ nơi xuất hiện$p_{i-1}$ĐẾN$p_i$, lợi ích từ việc mở rộng phân khúc hiện tại bằng$$2-(p_i-p_{i-1}).$$Thuật ngữ$2$đến từ việc đạt được thêm một lần xuất hiện, trong khi khoảng cách sẽ trừ đi những thất bại xảy ra giữa chúng. 
4. Nếu$$cur + (2-(p_i-p_{i-1}))$$tốt hơn là bắt đầu mới với điểm 1, hãy mở rộng phân khúc hiện tại. 
5. Nếu không, hãy bắt đầu một phân đoạn mới khi xuất hiện$p_i$. 
6. Sau khi cập nhật`cur`, so sánh nó với câu trả lời tốt nhất toàn cầu. Nếu nó lớn hơn, hãy lưu trữ: 

- giá trị$a$, 
- vị trí xuất hiện bắt đầu, 
- vị trí xuất hiện kết thúc. 
7. Lặp lại cho mọi giá trị riêng biệt. 

### Tại sao nó hoạt động 

Đối với một giá trị cố định$a$, mỗi phân đoạn ứng cử viên được xác định duy nhất bởi lần xuất hiện đầu tiên và cuối cùng của nó trong phân khúc đó. Việc mở rộng từ lần xuất hiện này sang lần xuất hiện tiếp theo sẽ thay đổi số mũ một cách chính xác$2-(p_i-p_{i-1})$. Tổng số mũ của một phân đoạn sẽ trở thành tổng của các mức tăng chuyển tiếp này cộng với đóng góp ban đầu là 1 từ lần xuất hiện đầu tiên. 

Điều đó biến bài toán thành việc tìm dãy con liền kề có tổng lớn nhất trong danh sách xuất hiện. Thuật toán của Kadane là tối ưu cho nhiệm vụ này. Vì chúng tôi giải quyết phân đoạn tối ưu một cách độc lập cho mọi giá trị và sau đó chọn kết quả tổng thể tốt nhất nên câu trả lời cuối cùng là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))

        pos = defaultdict(list)

        for i, x in enumerate(arr, start=1):
            pos[x].append(i)

        best_score = -10**18
        best_a = arr[0]
        best_l = 1
        best_r = 1

        for value, positions in pos.items():
            cur_score = 1
            cur_start = positions[0]

            if cur_score > best_score:
                best_score = cur_score
                best_a = value
                best_l = cur_start
                best_r = positions[0]

            for i in range(1, len(positions)):
                gain = 2 - (positions[i] - positions[i - 1])

                if cur_score + gain < 1:
                    cur_score = 1
                    cur_start = positions[i]
                else:
                    cur_score += gain

                if cur_score > best_score:
                    best_score = cur_score
                    best_a = value
                    best_l = cur_start
                    best_r = positions[i]

        out.append(f"{best_a} {best_l} {best_r}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên nhóm các giá trị bằng nhau và lưu trữ tất cả các vị trí của chúng. Đây là bước tiền xử lý quan trọng vì việc tối ưu hóa một giá trị chỉ phụ thuộc vào vị trí giá trị đó xuất hiện. 

Đối với mỗi danh sách lần xuất hiện,`cur_score`lưu trữ điểm tốt nhất trong số các phân đoạn kết thúc ở lần xuất hiện hiện tại. Độ lợi chuyển tiếp`2 - gap`xuất phát trực tiếp từ phép biến đổi đại số của số mũ. 

Điều kiện khởi động lại giống với thuật toán của Kadane. Nếu việc mở rộng tạo ra điểm dưới điểm của phân đoạn xảy ra một lần mới, chúng tôi sẽ loại bỏ phân đoạn trước đó và bắt đầu lại ở lần xuất hiện hiện tại. 

Các ranh giới được lưu trữ là các vị trí mảng thực tế, không phải các chỉ mục bên trong danh sách xuất hiện. Chi tiết này rất dễ bị nhầm lẫn. 

Không tồn tại vấn đề tràn số nguyên vì điểm số cao nhất$O(n)$và số nguyên Python dù sao cũng không bị giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
4 4 3 4 4
```Sự xuất hiện của giá trị 4: 

| Sự xuất hiện | Vị trí | Đạt được | Điểm hiện tại | Phân đoạn | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | - | 1 | [1,1] | 
| 2 | 2 | 1 | 2 | [1,2] | 
| 3 | 4 | 0 | 2 | [1,4] | 
| 4 | 5 | 1 | 3 | [1,5] | 

Điểm cuối cùng là 3, tương ứng với số mũ$$2\cdot4-5=3.$$Trả lời:```
4 1 5
```Dấu vết này cho thấy những lần xuất hiện gần đó tạo ra lợi ích tích cực như thế nào và duy trì ở cùng một phân khúc tối ưu. 

### Ví dụ 2 

đầu vào:```
5
11 1 11 1 11
```Lần xuất hiện của giá trị 11: 

| Sự xuất hiện | Vị trí | Đạt được | Điểm hiện tại | Phân đoạn | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | - | 1 | [1,1] | 
| 2 | 3 | 0 | 1 | [1,3] | 
| 3 | 5 | 0 | 1 | [1,5] | 

Lần xuất hiện của giá trị 1: 

| Sự xuất hiện | Vị trí | Đạt được | Điểm hiện tại | Phân đoạn | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | - | 1 | [2,2] | 
| 2 | 4 | 0 | 1 | [2,4] | 

Điểm tốt nhất chỉ là 1. Bất kỳ lần xuất hiện nào cũng là tối ưu. 

Điều này chứng tỏ rằng việc xảy ra nhiều lần không tự động có ích. Khoảng trống lớn có thể hủy bỏ hoàn toàn lợi ích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi vị trí mảng xuất hiện trong đúng một danh sách xuất hiện và được xử lý một lần | 
| Không gian |$O(n)$| Lưu trữ các vị trí xuất hiện | 

Tổng cộng$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$. Quét tuyến tính tất cả các danh sách xuất hiện dễ dàng nằm trong giới hạn 2 giây và việc lưu trữ tất cả các vị trí chỉ cần bộ nhớ tuyến tính. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))

        pos = defaultdict(list)

        for i, x in enumerate(arr, 1):
            pos[x].append(i)

        best_score = -10**18
        ans = None

        for value, positions in pos.items():
            cur = 1
            start = positions[0]

            if cur > best_score:
                best_score = cur
                ans = (value, start, positions[0])

            for i in range(1, len(positions)):
                gain = 2 - (positions[i] - positions[i - 1])

                if cur + gain < 1:
                    cur = 1
                    start = positions[i]
                else:
                    cur += gain

                if cur > best_score:
                    best_score = cur
                    ans = (value, start, positions[i])

        out.append(f"{ans[0]} {ans[1]} {ans[2]}")

    return "\n".join(out)

# minimum size
assert run("1\n1\n5\n") == "5 1 1"

# all equal
assert run("1\n5\n7 7 7 7 7\n") == "7 1 5"

# isolated repeats, best is single element
assert run("1\n5\n7 1 1 1 7\n") in {
    "7 1 1",
    "7 5 5",
    "1 2 2",
    "1 3 3",
    "1 4 4",
}

# adjacent occurrences should merge
assert run("1\n4\n9 9 9 9\n") == "9 1 4"

# off-by-one boundary
assert run("1\n5\n3 1 3 3 1\n") == "3 1 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[5]`|`5 1 1`| Mảng kích thước tối thiểu | 
|`[7,7,7,7,7]`|`7 1 5`| Toàn bộ mảng là tối ưu | 
|`[7,1,1,1,7]`| Bất kỳ phân đoạn xuất hiện đơn lẻ nào | Khởi động lại logic | 
|`[9,9,9,9]`|`9 1 4`| Mức tăng tích cực liên tiếp | 
|`[3,1,3,3,1]`|`3 1 4`| Xử lý ranh giới đúng | 

## Vỏ cạnh 

### Một giá trị chỉ xuất hiện một lần 

đầu vào:```
1
1
100
```Danh sách xảy ra là`[1]`. 

Thuật toán khởi tạo: 

| Vị trí | Điểm | 
| --- | --- | 
| 1 | 1 | 

Không có chuyển tiếp tồn tại. Câu trả lời trở thành:```
100 1 1
```Giải pháp giả định mọi phân đoạn tối ưu đều chứa nhiều lần xuất hiện sẽ không thành công ở đây. 

### Các giá trị lặp lại được phân tách bằng khoảng trống lớn 

đầu vào:```
1
5
7 1 1 1 7
```Đối với giá trị 7: 

| Sự xuất hiện | Vị trí | Đạt được | Điểm | 
| --- | --- | --- | --- | 
| 1 | 1 | - | 1 | 
| 2 | 5 | -2 | khởi động lại | 

Điểm mở rộng sẽ âm nên Kadane bắt đầu lại ở vị trí thứ 5. 

Thuật toán trả về chính xác một phân đoạn vị trí đơn vì việc kết nối hai lần xuất hiện gây ra quá nhiều tổn thất. 

### Đoạn tối ưu bắt đầu ở giữa 

đầu vào:```
1
7
5 1 1 5 5 1 5
```Lần xuất hiện của 5 là ở các vị trí:```
1 4 5 7
```Khoảng cách đầu tiên là lớn và tạo ra sự đóng góp tiêu cực. Việc khởi động lại Kadane sẽ loại bỏ sự xuất hiện sớm và bắt đầu một phân đoạn tốt hơn sau đó. 

Đây chính xác là lý do tại sao thuật toán lưu trữ phân đoạn hiện tại bắt đầu tách biệt với lần xuất hiện đầu tiên của giá trị. Nếu không có cơ chế khởi động lại đó, câu trả lời sẽ là dưới mức tối ưu.
