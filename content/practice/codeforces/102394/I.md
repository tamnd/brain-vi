---
title: "CF 102394I - Hoán vị thú vị"
description: "Chúng ta có một hoán vị (a) chứa mọi số nguyên từ (1) đến (n) đúng một lần. Với mỗi tiền tố (a1,ldots,ai), hãy xem xét các giá trị lớn nhất và nhỏ nhất của nó. Sự khác biệt của họ là (xin chào)."
date: "2026-08-10T21:24:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102394
codeforces_index: "I"
codeforces_contest_name: "The 2019 China Collegiate Programming Contest Harbin Site"
rating: 0
weight: 102394
solve_time_s: 97
verified: true
draft: false
---

[CF 102394I - Hoán vị thú vị](https://codeforces.com/problemset/problem/102394/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hoán vị (a) chứa mọi số nguyên từ (1) đến (n) đúng một lần. Đối với mỗi tiền tố (a_1,\ldots,a_i), hãy xem xét các giá trị lớn nhất và nhỏ nhất của nó. Sự khác biệt của họ là (h_i). Nhiệm vụ là xác định có bao nhiêu hoán vị khác nhau tạo ra chính xác chuỗi đã cho (h), với kết quả lấy modulo (10^9+7). 

Thực tế quan trọng là (h_i) chỉ mô tả độ rộng của khoảng giữa mức tối thiểu và tối đa hiện tại. Nó không cho chúng tôi biết điểm cuối nào là tối thiểu hoặc tối đa cũng như vị trí xuất hiện các giá trị nghiêm ngặt giữa các điểm cuối đó. Do đó, vấn đề đếm là có bao nhiêu cách chúng ta có thể tăng khoảng này trong khi đặt mỗi phần tử hoán vị đúng một lần. 

Kho lưu trữ chính thức đưa ra giới hạn thời gian 1 giây và giới hạn bộ nhớ 512 MB. Giá trị của (n) có thể đạt tới (10^5) và tổng (n) trên tất cả các trường hợp thử nghiệm có thể đạt tới (2\cdot10^6). Một giải pháp bậc hai theo (n) sẽ thực hiện khoảng (10^{10}) phép toán trong đầu vào tổng hợp lớn nhất, vượt xa giới hạn dự định. Thậm chí (O(n\log n)) ở đây là không cần thiết vì thông tin cần thiết ở vị trí (i) chỉ phụ thuộc vào chiều rộng trước đó và số lượng giá trị bên trong hiện không được sử dụng. Mục tiêu là quét tuyến tính. 

Có một số trình tự không hợp lệ mà việc triển khai bất cẩn có thể xử lý sai. Đầu tiên, (h_1) phải bằng 0 vì tiền tố đầu tiên chỉ chứa một giá trị, do đó giá trị tối đa và tối thiểu của nó bằng nhau. Ví dụ: với (n=2) và (h=[1,1]), câu trả lời đúng là (0). Việc triển khai chỉ bắt đầu xử lý từ (i=2) mà không kiểm tra (h_1) có thể chấp nhận nó một cách không chính xác. 

Thứ hai, chiều rộng không bao giờ có thể giảm. Khi một giá trị đã được nhìn thấy, tiền tố tối thiểu chỉ có thể giữ nguyên hoặc giảm đi, trong khi tiền tố tối đa chỉ có thể giữ nguyên hoặc tăng. Như vậy (h_i\ge h_{i-1}). Ví dụ: (n=3) và (h=[0,2,1]) có câu trả lời (0). Một phép lặp bất cẩn coi mọi chuyển đổi không bằng nhau là một phép khai triển sẽ tính mức giảm không thể xảy ra này là một phép khai triển hợp lệ khác. 

Thứ ba, chiều rộng không được vượt quá (n-1), vì tất cả các giá trị hoán vị đều nằm trong khoảng ([1,n]). Ví dụ: (n=3) và (h=[0,3,3]) có đáp án (0). Sự khác biệt lớn nhất có thể là (3-1=2). 

Cuối cùng, việc ổn định có thể là không thể xảy ra mặc dù dãy số không giảm. Xét (n=3) và (h=[0,1,1]). Sau lần mở rộng đầu tiên, tiền tố phải chứa hai điểm cuối của một khoảng có chiều rộng (1), chẳng hạn như (1,2) hoặc (2,3). Hoàn toàn không có giá trị nào được sử dụng giữa các điểm cuối đó, vì vậy vị trí thứ ba không có lựa chọn hợp pháp. Câu trả lời đúng là (0). Một giải pháp đơn giản nhân với một số cố định cho mỗi lần chuyển đổi bằng nhau có thể bỏ sót việc thiếu các giá trị bên trong sẵn có này. 

Ở một thái cực khác, (n=1) với (h=[0]) là hợp lệ và có chính xác một hoán vị, đó là ([1]). Trường hợp này rất hữu ích vì không có quá trình chuyển đổi nào sang quy trình, vì vậy câu trả lời ban đầu phải là (1). 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi hoán vị của (1,\ldots,n). Đối với mỗi hoán vị, hãy quét nó từ trái sang phải, duy trì tiền tố tối đa và tối thiểu của nó và so sánh sự khác biệt thu được với (h) đã cho. Điều này đúng vì mọi hoán vị có thể đều được kiểm tra chính xác một lần. 

Vấn đề là số lượng hoán vị. Có (n!) Trong số chúng và việc kiểm tra một hoán vị sẽ mất (\Theta(n)) thời gian. Do đó, tổng độ phức tạp là (\Theta(n\cdot n!)). Ngay cả ở (n=10), điều này đã có nghĩa là khoảng (3,6\times10^7) hoạt động tiền tố, trong khi (n) được phép là (10^5). Phương pháp vũ phu không chỉ hơi chậm mà còn không thể sử dụng được. 

Quan sát hữu ích là chúng ta không bao giờ cần biết chính xác giá trị tối thiểu và tối đa. Chúng ta chỉ cần biết khoảng cách của họ thay đổi như thế nào.

Giả sử tiền tố hiện tại có chiều rộng (h_{i-1}). Vì khoảng tiền tố không thể co lại nên (h_i<h_{i-1}) ngay lập tức là không thể. 

Nếu (h_i>h_{i-1}), giá trị mới (a_i) phải trở thành mức tối đa mới hoặc mức tối thiểu mới. Đó là hai khả năng duy nhất, do đó quá trình chuyển đổi này đóng góp hệ số (2). Nếu chiều rộng tăng thêm (d=h_i-h_{i-1}), thì điểm cuối mới sẽ để lại chính xác (d-1) các giá trị chưa từng thấy trước đó bên trong khoảng mới. Các giá trị này trở thành lựa chọn trong tương lai cho các vị trí có chiều rộng không thay đổi. 

Nếu (h_i=h_{i-1}), giá trị mới không thể là giá trị tối đa hoặc tối thiểu mới. Nó phải là một giá trị không được sử dụng hoàn toàn giữa các điểm cuối hiện tại. Như vậy số lựa chọn chính xác là số giá trị bên trong chưa được sử dụng. 

Điều này mang lại một trạng thái rất nhỏ. Cho phép`slots`là số lượng giá trị không được sử dụng hiện nằm giữa tiền tố tối thiểu và tối đa. Khi khoảng mở rộng thêm (d), chúng ta thêm (d-1) các giá trị bên trong mới. Khi khoảng không thay đổi, chúng ta chọn một trong các`slots`giá trị và loại bỏ nó, vì vậy câu trả lời được nhân với`slots`và sau đó`slots`giảm đi một. 

Lực lượng vũ phu hoạt động vì mỗi hoán vị xác định một chuỗi mở rộng điểm cuối và vị trí bên trong duy nhất. Nó thất bại vì nó khám phá tất cả các hoán vị một cách độc lập. Quan sát rằng thông tin liên quan duy nhất là chiều rộng hiện tại và số lượng giá trị bên trong không được sử dụng cho phép chúng tôi đếm đồng thời tất cả các hoán vị có cùng lịch sử. 

Các cách tiếp cận kết quả là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n\cdot n!)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) cho mảng đầu vào, (O(1)) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n) và dãy (h). Chuỗi hợp lệ phải bắt đầu bằng (h_1=0), vì tiền tố đầu tiên chỉ chứa một giá trị. Ngoài ra, mọi (h_i) tối đa phải bằng (n-1) và giá trị cuối cùng phải là (n-1), vì sau khi tất cả (n) giá trị được chèn vào, tiền tố sẽ chứa cả (1) và (n). Nếu bất kỳ điều kiện nào trong số này không thành công, câu trả lời là không. 
2. Quét chuỗi từ (i=2) đến (n), giữ nguyên`ans`, số lượng hoán vị một phần được biểu thị bằng trạng thái hiện tại và`slots`, số lượng giá trị không được sử dụng hoàn toàn nằm trong khoảng tối thiểu và tối đa hiện tại. 
3. Nếu (h_i<h_{i-1}), trả về 0. Khoảng tiền tố chỉ có thể giữ nguyên hoặc mở rộng chứ không bao giờ co lại. 
4. Nếu (h_i>h_{i-1}), phần tử mới phải mở rộng một trong hai điểm cuối. Nó có thể trở thành mức tối đa mới hoặc mức tối thiểu mới, cho chính xác hai lựa chọn, vì vậy hãy nhân lên`ans`bởi (2). 
5. Đặt (d=h_i-h_{i-1}). Phía mới được mở rộng có (d) các vị trí có chiều rộng được thêm vào, nhưng bản thân điểm cuối lại bị (a_i) chiếm giữ ngay lập tức. Các giá trị (d-1) còn lại hoàn toàn nằm trong khoảng mới và chưa xuất hiện trước đó, do đó hãy tăng`slots`bởi (d-1). 
6. Nếu (h_i=h_{i-1}), phần tử mới phải nằm hoàn toàn bên trong khoảng hiện tại. Có chính xác`slots`giá trị không sử dụng có sẵn, vì vậy hãy nhân lên`ans`qua`slots`và giảm`slots`bởi một. Nếu như`slots`bằng 0, không hoán vị nào có thể thực hiện được quá trình chuyển đổi này và câu trả lời trở thành 0. 
7. Thực hiện mọi modulo nhân (10^9+7), sau đó in kết quả cuối cùng. 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý vị trí (i),`slots`chính xác là số giá trị chưa xuất hiện trong hoán vị và nằm hoàn toàn giữa tiền tố tối thiểu và tối đa hiện tại. Khi chiều rộng tăng lên, điểm cuối mới buộc phải ở mức tối thiểu hoặc tối đa, đưa ra hai lựa chọn và mọi số nguyên bên trong mới được tạo sẽ không được sử dụng vì tất cả các giá trị trước đó đều nằm trong khoảng cũ. Có chính xác (h_i-h_{i-1}-1) giá trị như vậy. Khi chiều rộng không thay đổi, giá trị tiếp theo phải là một trong những giá trị bên trong không được sử dụng này và mọi lựa chọn như vậy sẽ bảo toàn cả khoảng hiện tại và tất cả các giá trị (h) trước đó. Do đó, mọi hoán vị hợp lệ sẽ được tính chính xác một lần, trong khi mỗi lần chuyển đổi được tính sẽ tương ứng với một vị trí hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        h = list(map(int, input().split()))

        if h[0] != 0 or h[-1] != n - 1:
            out.append("0")
            continue

        valid = True
        for i in range(1, n):
            if h[i] < h[i - 1] or h[i] > n - 1:
                valid = False
                break

        if not valid:
            out.append("0")
            continue

        ans = 1
        slots = 0

        for i in range(1, n):
            if h[i] == h[i - 1]:
                if slots == 0:
                    ans = 0
                    break

                ans = ans * slots % MOD
                slots -= 1
            else:
                diff = h[i] - h[i - 1]

                ans = ans * 2 % MOD
                slots += diff - 1

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Kiểm tra xác thực đầu tiên (h_1=0) và (h_n=n-1). Điều kiện đầu tiên mô tả tiền tố một phần tử, trong khi điều kiện thứ hai xuất phát từ thực tế là hoán vị hoàn chỉnh chứa cả (1) và (n). 

Kiểm tra tính đơn điệu sẽ loại bỏ chiều rộng giảm ngay lập tức. Việc kiểm tra giới hạn trên được ngụ ý về mặt kỹ thuật bởi (h_n=n-1) cùng với tính đơn điệu, nhưng việc giữ nó rõ ràng sẽ làm cho các điều kiện hợp lệ trở nên rõ ràng và bảo vệ logic chuyển đổi khỏi các giá trị đầu vào không thể thực hiện được.`slots`bắt đầu từ số không. Lúc đầu chỉ có một phần tử hoán vị, do đó không thể có giá trị không được sử dụng nằm giữa giá trị tối thiểu và tối đa. 

Đối với một quá trình chuyển đổi mở rộng,`diff`là tích cực. Nhân với hai tài khoản để chọn xem phần tử mới trở thành tối thiểu hay tối đa. biểu thức`diff - 1`đếm các giá trị nội thất mới. Khi`diff`là một, không có giá trị bên trong nào được tạo ra, đó là lý do tại sao bản cập nhật này có thể thêm số 0 một cách chính xác. 

Để có sự chuyển đổi bình đẳng,`slots`là số lượng chính xác các lựa chọn hợp pháp. Kiểm tra`slots == 0`trước khi nhân tránh làm cho trạng thái âm. Số lượng vị trí âm sẽ không có ý nghĩa tổ hợp và có thể gây ra câu trả lời mô-đun không chính xác. 

Số nguyên Python không bị tràn, nhưng phép toán modulo vẫn được yêu cầu vì câu trả lời tăng theo cấp số nhân. Tổng kích thước đầu vào tối đa là (2\cdot10^6), do đó việc lưu trữ một mảng (h) cho mỗi trường hợp thử nghiệm và xử lý nó một lần là điều dễ dàng quản lý. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trường hợp mẫu đầu tiên là (n=3) với (h=[0,2,2]). 

| Vị trí | (h_i) | Trước (h) | Chuyển tiếp |`ans`|`slots`| 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | Trạng thái ban đầu | 1 | 0 | 
| 2 | 2 | 0 | Mở rộng thêm 2 | 2 | 1 | 
| 3 | 2 | 2 | Sử dụng một giá trị nội thất | 2 | 0 | 

Ở vị trí 2, chiều rộng nhảy từ 0 lên 2. Giá trị mới có thể là mức tối thiểu mới hoặc mức tối đa mới, đưa ra hai khả năng. Mức tăng chiều rộng là hai, do đó, chính xác một giá trị mới nằm chính xác giữa các điểm cuối. Ở vị trí 3 giá trị đó là bắt buộc. Hai hoán vị thu được là ([1,3,2]) và ([3,1,2]), đưa ra câu trả lời (2). 

### Mẫu 2 

Trường hợp mẫu thứ hai là (n=3) với (h=[0,1,2]). 

| Vị trí | (h_i) | Trước (h) | Chuyển tiếp |`ans`|`slots`| 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | Trạng thái ban đầu | 1 | 0 | 
| 2 | 1 | 0 | Mở rộng thêm 1 | 2 | 0 | 
| 3 | 2 | 1 | Mở rộng thêm 1 | 4 | 0 | 

Mỗi lần chuyển đổi sẽ mở rộng khoảng thời gian thêm chính xác một. Mỗi bản mở rộng có hai lựa chọn, nhưng không tạo ra giá trị nội tại không được sử dụng vì`diff - 1`là số không. Do đó câu trả lời là (2\cdot2=4). Bốn hoán vị là ([1,2,3]), ([2,1,3]), ([2,3,1]) và ([3,2,1]). 

Hai ví dụ này cho thấy hai loại chuyển tiếp cơ bản. Cái đầu tiên tạo một khe bên trong và sau đó sử dụng nó, trong khi cái thứ hai chỉ mở rộng các điểm cuối và không bao giờ cần vị trí bên trong. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) trên mỗi trường hợp thử nghiệm, (O(\sum n)) tổng thể | Trình tự được quét một số lần không đổi. | 
| Không gian | (O(n)) | Trình tự đầu vào được lưu trữ; trạng thái đếm sử dụng thêm (O(1)) không gian. | 

Vì tổng của tất cả (n) tối đa là (2\cdot10^6), nên tổng số lần lặp vòng lặp là tuyến tính ở kích thước đầu vào hoàn chỉnh. Điều này phù hợp với các ràng buộc dự định, trong khi các phương pháp bậc hai hoặc giai thừa thì không. Giới hạn vấn đề chính thức là 1 giây và 512 MB. 

## Trường hợp thử nghiệm```python
import io
import sys

MOD = 10**9 + 7

def solve_data(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        h = list(map(int, input().split()))

        if h[0] != 0 or h[-1] != n - 1:
            out.append("0")
            continue

        valid = True
        for i in range(1, n):
            if h[i] < h[i - 1] or h[i] > n - 1:
                valid = False
                break

        if not valid:
            out.append("0")
            continue

        ans = 1
        slots = 0

        for i in range(1, n):
            if h[i] == h[i - 1]:
                if slots == 0:
                    ans = 0
                    break
                ans = ans * slots % MOD
                slots -= 1
            else:
                diff = h[i] - h[i - 1]
                ans = ans * 2 % MOD
                slots += diff - 1

        out.append(str(ans))

    return "\n".join(out)

# Provided samples
sample = """\
3
3
0 2 2
3
0 1 2
3
0 2 3
"""
assert solve_data(sample) == "2\n4\n0", "provided samples"

# Minimum-size input
assert solve_data("""\
1
1
0
""") == "1", "n=1 has exactly one permutation"

# All equal values, impossible for n > 1
assert solve_data("""\
1
4
0 0 0 0
""") == "0", "no distinct permutation can keep width zero"

# Boundary case with exactly one interior slot
assert solve_data("""\
1
3
0 2 2
""") == "2", "one forced interior placement"

# Decreasing width
assert solve_data("""\
1
3
0 2 1
""") == "0", "prefix width cannot decrease"

# Width larger than n - 1
assert solve_data("""\
1
3
0 3 3
""") == "0", "maximum possible width is n-1"

# Equal transition with no available interior slot
assert solve_data("""\
1
3
0 1 1
""") == "0", "plateau cannot be filled"

# Maximum-size input.
# h = [0, 1, 2, ..., n-1], so every transition has two choices.
n = 100000
h = list(range(n))
inp = "1\n{}\n{}\n".format(n, " ".join(map(str, h)))
expected = str(pow(2, n - 1, MOD))
assert solve_data(inp) == expected, "maximum-size linear scan"
```Các trường hợp tùy chỉnh xác thực hoán vị nhỏ nhất có thể, không thể giữ chiều rộng bằng 0 sau phần tử đầu tiên, việc tạo và sử dụng một khe bên trong, giảm chiều rộng, chiều rộng ngoài phạm vi được hoán vị cho phép, một điểm cố định không có khe cắm sẵn có và kích thước đầu vào tối đa được phép. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 0`|`1`| Kích thước tối thiểu và vòng chuyển tiếp trống | 
|`1 / 4 / 0 0 0 0`|`0`| Độ rộng hoàn toàn bằng nhau cho (n>1) | 
|`1 / 3 / 0 2 2`|`2`| Đếm khe nội thất | 
|`1 / 3 / 0 2 1`|`0`| Giảm chiều rộng | 
|`1 / 3 / 0 3 3`|`0`| Chiều rộng lớn hơn (n-1) | 
|`1 / 3 / 0 1 1`|`0`| Chuyển đổi bình đẳng với các vị trí bằng 0 | 
|`1 / 100000 / 0 1 \ldots 99999`| (2^{99999}\bmod(10^9+7)) | Xử lý tuyến tính kích thước tối đa | 

## Vỏ cạnh 

Đối với (n=1), hoán vị duy nhất có thể là ([1]), do đó chuỗi được tạo ra là ([0]). Thuật toán chấp nhận (h_1=0), xác nhận (h_n=n-1=0), không thực hiện chuyển đổi và trả về câu trả lời ban đầu (1). 

Đối với giá trị đầu tiên không hợp lệ, hãy xem xét (n=2) và (h=[1,1]). Tiền tố một phần tử luôn có giá trị tối đa bằng tối thiểu, do đó chiều rộng của nó phải bằng 0. Xác thực ban đầu từ chối trình tự trước khi quét chuyển tiếp, đưa ra (0). 

Đối với dãy giảm dần, hãy xem xét (n=3) và (h=[0,2,1]). Sau khi hai phần tử đầu tiên tạo ra chiều rộng (2), việc thêm phần tử khác không thể làm cho phạm vi tiền tố thu hẹp lại. Quá trình quét phát hiện (1<2) và trả về (0). 

Đối với chiều rộng quá lớn, hãy xem xét (n=3) và (h=[0,3,3]). Các giá trị duy nhất có sẵn là (1,2,3), vì vậy chênh lệch lớn nhất có thể là (2). Vì (h_2=3>n-1), dãy bị bác bỏ và đáp án là (0). 

Đối với một cao nguyên không có sẵn giá trị bên trong, hãy xem xét (n=3) và (h=[0,1,1]). Bản mở rộng đầu tiên có`diff=1`, do đó nó tạo ra`diff-1=0`các khe bên trong. Ở vị trí tiếp theo chiều rộng vẫn bằng nhau, nhưng`slots`bằng 0, do đó không có giá trị pháp lý nào được đặt và câu trả lời trở thành (0). 

Đối với một cao nguyên có giá trị bên trong sẵn có, hãy xem xét (n=3) và (h=[0,2,2]). Việc mở rộng có`diff=2`, tạo một khe bên trong. Quá trình chuyển đổi bằng nhau sẽ nhân câu trả lời với một và chiếm hết vị trí đó. Hai hướng điểm cuối vẫn khác biệt, đưa ra câu trả lời (2). 

Để có chiều rộng lớn nhất có thể, hãy xem xét (h=[0,1,2,\ldots,n-1]). Mỗi quá trình chuyển đổi sẽ mở rộng khoảng thời gian thêm một, do đó luôn có chính xác hai lựa chọn và không có ô bên trong nào được tạo. Kết quả là (2^{n-1}\bmod(10^9+7)), cũng cung cấp thử nghiệm hiệu suất kích thước tối đa hữu ích.
