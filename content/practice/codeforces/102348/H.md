---
title: "CF 102348H - Triển vọng Berland"
description: "Chúng ta có n đèn lồng được đặt ở tọa độ nguyên tăng dần x[0], x[1], ..., x[n-1]. Chúng tôi có thể bật bất kỳ tập hợp con nào trong số chúng. Các tọa độ được chọn phải tạo thành một cấp số cộng, nghĩa là mọi tọa độ được chọn liên tiếp đều có cùng sự khác biệt."
date: "2026-08-15T17:27:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "H"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 242
verified: false
draft: false
---

[CF 102348H - Triển vọng Berland](https://codeforces.com/problemset/problem/102348/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`đèn lồng được đặt ở tọa độ nguyên tăng dần`x[0], x[1], ..., x[n-1]`. Chúng tôi có thể bật bất kỳ tập hợp con nào trong số chúng. Các tọa độ được chọn phải tạo thành một cấp số cộng, nghĩa là mọi tọa độ được chọn liên tiếp đều có cùng sự khác biệt. Bất kỳ tập hợp một hoặc hai đèn lồng nào đều tự động hợp lệ, vì vậy nhiệm vụ thực sự là tìm ra dãy con lũy tiến số học dài nhất của mảng tọa độ đã sắp xếp. 

Các ràng buộc ban đầu đưa ra`n <= 3000`và phối hợp lên đến`10^18`. Giới hạn tọa độ lớn loại trừ các kỹ thuật phụ thuộc vào phạm vi tọa độ nhỏ, nhưng nó không ảnh hưởng đến bản thân số học vì số nguyên Python xử lý các giá trị này một cách chính xác. Kích thước`3000`là hạn chế chính. MỘT`O(n^2)`Thuật toán thực hiện khoảng 4,5 triệu chuyển đổi cặp, phù hợp với giới hạn 2 giây khi được thực hiện cẩn thận. MỘT`O(n^3)`thuật toán đã có khoảng 27 tỷ lần lặp trong giới hạn trường hợp xấu nhất, vượt xa giới hạn. Tuyên bố chính thức xác nhận những giới hạn này và thứ tự tọa độ tăng dần một cách nghiêm ngặt. 

Có một số trường hợp việc triển khai có thể diễn ra sai sót một cách âm thầm. Đầu tiên, ba chiếc đèn lồng tùy ý không nhất thiết tạo thành một tiến trình hợp lệ. Ví dụ,`3 5 9`chỉ có hai khoảng trống bằng nhau,`2`Và`4`, vậy câu trả lời là`2`, không`3`. Việc triển khai bất cẩn coi mọi tiện ích mở rộng cặp là hợp lệ tự động sẽ bị tính quá mức. 

Thứ hai, tiến trình tốt nhất có thể bỏ qua nhiều đèn lồng. Vì`1 2 4 6 7`, câu trả lời là`3`, sử dụng`1, 4, 7`. Chỉ nhìn vào những chiếc đèn lồng liên tiếp sẽ bỏ lỡ tiến trình này vì những khoảng trống ban đầu đã bị bỏ qua.`1, 2, 2, 1`. 

Thứ ba, tọa độ đầu vào có thể gần với`10^18`. Ví dụ,`0 500000000000000000 1000000000000000000`có câu trả lời`3`. Việc triển khai có chiều rộng cố định phải sử dụng loại đủ rộng cho các biểu thức như`2*x[i]`, mặc dù các số nguyên có độ chính xác tùy ý của Python khiến vấn đề này trở nên đơn giản. 

Cuối cùng, cụm từ "tất cả các giá trị bằng nhau" không thể tạo ra một phép thử hợp lệ theo các ràng buộc đầu vào của bài toán này theo đúng nghĩa đen vì tọa độ đang tăng lên một cách nghiêm ngặt. Phiên bản có ý nghĩa của trường hợp căng thẳng đó là một mảng có tất cả các khoảng trống bằng nhau, chẳng hạn như`0 1 2 3 4`, trong đó mỗi tọa độ thuộc về một cấp số cộng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp có thể chọn đèn lồng thứ nhất và thứ hai, xác định sự khác biệt của chúng và sau đó tìm kiếm mọi tọa độ tiếp theo tiếp tục tiến trình tương tự. Nếu chúng ta quét tất cả các vị trí tiếp theo có thể một cách ngây thơ, sẽ có`O(n^2)`sự lựa chọn của hai chiếc đèn lồng đầu tiên trở lên`O(n)`làm việc cho mỗi người, đưa ra`O(n^3)`thời gian. Tương tự, kiểm tra từng bộ ba đã có nghĩa là kiểm tra`C(3000, 3) = 4,495,501,000`gấp ba lần trong trường hợp xấu nhất. Lực lượng vũ phu là chính xác bởi vì mọi cấp số cộng đều có cặp đầu tiên, do đó, việc thử từng cặp cuối cùng sẽ xem xét sự khác biệt chung chính xác của nó. Nó chỉ đơn giản là lặp lại quá nhiều công việc. 

Quan sát hữu ích là khi biết được hai tọa độ được chọn cuối cùng, tọa độ tiếp theo sẽ được xác định hoàn toàn. Giả sử một cấp số cộng hiện kết thúc ở tọa độ`x[h], x[i]`. Tọa độ tiếp theo của nó phải là`x[j] = x[i] + (x[i] - x[h]) = 2*x[i] - x[h]`. 

Điều đó biến vấn đề thành một chương trình động trên các cặp điểm cuối. Cho phép`dp[i][j]`là độ dài tối đa của một cấp số cộng có hai tọa độ cuối cùng là`x[i]`Và`x[j]`, với`i < j`. Nếu yêu cầu tọa độ tiền nhiệm`2*x[i] - x[j]`tồn tại ở chỉ mục`h`, thì tiến trình có thể được mở rộng từ trạng thái kết thúc tại`(h, i)`:`dp[i][j] = dp[h][i] + 1`. 

Nếu tiền thân đó không tồn tại, cặp`(i, j)`chính nó là một cấp số hợp lệ của độ dài`2`, Vì thế`dp[i][j] = 2`. 

Thứ tự sắp xếp cung cấp thêm một thuộc tính hữu ích. Bởi vì`j > i`, tiền thân bắt buộc`2*x[i] - x[j]`thực sự nhỏ hơn`x[i]`, vậy chỉ số của nó`h`phải thỏa mãn`h < i`. Tất cả thông tin cần thiết cho quá trình chuyển đổi đã được tính toán khi xử lý`i`. 

Chúng ta có thể tìm thấy`h`không có bảng băm. Đối với một cố định`i`, BẰNG`j`tăng lên,`x[j]`tăng lên, do đó`2*x[i] - x[j]`giảm đi. Do đó, con trỏ quét lùi qua tọa độ chỉ di chuyển lùi trong toàn bộ vòng lặp bên trong. Trên một cố định`i`, con trỏ di chuyển nhiều nhất`i`lần, do đó tổng số chuyển động của con trỏ trên tất cả`i`là`O(n^2)`. 

DP chứa`n(n-1)/2`cặp trạng thái. Trong Python, việc lưu trữ từng trạng thái dưới dạng số nguyên bình thường bên trong các danh sách lồng nhau có thể tiêu tốn một lượng bộ nhớ đáng kể. Giá trị DP tối đa là nhiều nhất`3000`, do đó, số nguyên 16 bit không dấu là đủ. của Python`array('H')`lưu trữ mỗi trạng thái trong hai byte, giữ khoảng 4,5 triệu trạng thái trong một vùng nhớ nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^3)`|`O(1)`hoặc`O(n)`| Quá chậm | 
| Ghép nối DP với con trỏ tiền nhiệm đơn điệu |`O(n^2)`|`O(n^2)`tiểu bang, mỗi tiểu bang khoảng 2 byte | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tọa độ đã sắp xếp và khởi tạo câu trả lời cho`2`. Mỗi cặp đèn lồng riêng biệt đều là một lựa chọn hợp lệ, vì vậy câu trả lời luôn ít nhất là`2`. 
2. Đối với mọi chỉ số`i`, tạo một hàng DP chứa các trạng thái`dp[i][j]`cho tất cả`j > i`. Khởi tạo mọi trạng thái thành`2`, bởi vì bất kỳ cặp đèn lồng nào cũng tạo thành một cấp số cộng có độ dài bằng hai. 
3. Cố định điểm cuối giữa`i`và bắt đầu một con trỏ tiền nhiệm`k = i - 1`. Đối với mọi`j > i`, tọa độ yêu cầu ngay trước`x[i]`là`target = 2*x[i] - x[j]`. 
4. Di chuyển`k`còn lại trong khi`x[k] > target`. Từ`target`chỉ giảm khi`j`tăng lên,`k`không bao giờ cần phải di chuyển sang phải nữa. Nếu như`x[k] == target`, chỉ số`k`chính xác là người tiền nhiệm được yêu cầu. 
5. Khi tồn tại trạng thái tiền nhiệm, hãy đọc trạng thái đã được tính toán kết thúc tại`(k, i)`và thiết lập`dp[i][j] = dp[k][i] + 1`. Trạng thái trước đó có ít phần tử hơn và nối thêm`x[j]`duy trì sự khác biệt chung bởi vì`x[i] - x[k] = x[j] - x[i]`. 
6. Cập nhật câu trả lời chung với trạng thái mới. Nếu giá trị trước không tồn tại, giá trị khởi tạo`2`vẫn đúng. 
7. Xử lý mọi việc có thể`i`. Cuối cùng, giá trị DP lớn nhất là dãy con lũy tiến số học dài nhất và là câu trả lời bắt buộc. 

### Tại sao nó hoạt động 

Xét mọi cấp số cộng kết thúc tại`x[i], x[j]`. Tọa độ trước đó của nó, nếu có thì phải chính xác`2*x[i] - x[j]`. Bởi vì tọa độ được sắp xếp và`j > i`, tiền thân đó nằm trước`i`. Do đó, mọi cấp số nhân có độ dài ít nhất bằng ba đều có một trạng thái DP sớm hơn duy nhất mà từ đó nó có thể được mở rộng. Phép truy toán xem xét chính xác tiền thân đó khi nó tồn tại và nếu không thì sẽ rời khỏi cặp một cách chính xác ở độ dài hai. Bằng cách quy nạp vào vị trí của điểm cuối cuối cùng, mọi trạng thái DP sẽ lưu trữ tiến trình hợp lệ tối đa kết thúc ở cặp đó. Do đó, việc lấy mức tối đa trên tất cả các cặp sẽ mang lại mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n = int(input())
    x = list(map(int, input().split()))

    if n <= 2:
        print(n)
        return

    # rows[i][j - i - 1] stores dp[i][j] for j > i.
    # Every state starts at 2 because every pair is valid.
    rows = [None] * n
    answer = 2

    for i in range(n - 1):
        length = n - i - 1
        row = array('H', [2]) * length

        # For fixed i, target = 2*x[i] - x[j] decreases as j grows.
        # Hence k only moves to the left.
        k = i - 1
        xi = x[i]

        for j in range(i + 1, n):
            target = 2 * xi - x[j]

            while k >= 0 and x[k] > target:
                k -= 1

            if k >= 0 and x[k] == target:
                value = rows[k][i - k - 1] + 1
                row[j - i - 1] = value

                if value > answer:
                    answer = value

        rows[i] = row

    print(answer)

if __name__ == "__main__":
    solve()
```các`rows`cấu trúc chỉ lưu trữ các trạng thái có`i < j`. Đối với một cố định`i`, tiểu bang`dp[i][j]`được lưu trữ ở offset`j - i - 1`, do đó hàng chứa chính xác`n-i-1`mục nhập. 

biểu thức`rows[k][i-k-1]`lấy lại`dp[k][i]`. Phần bù rất dễ bị sai vì mỗi hàng bắt đầu ở điểm cuối thứ hai hợp lệ đầu tiên của chính nó. Ví dụ, ở hàng`k`, trạng thái có chỉ số thứ hai là`i`là`(i-k-1)`-phần tử thứ. 

Con trỏ đứng trước bắt đầu tại`i-1`, chỉ số tiền thân lớn nhất có thể. Đối với mỗi cái mới`j`, tọa độ cần thiết giảm đi. các`while`vòng lặp di chuyển con trỏ cho đến khi tìm thấy tọa độ chính xác hoặc mọi phần trước có thể trở nên quá lớn. Nó không bao giờ cần phải khởi động lại từ`i-1`, đó là điều giữ cho việc tìm kiếm ở dạng bậc hai thay vì bậc ba. 

các`array('H')`loại là đủ vì không có tiến trình nào có thể chứa nhiều hơn`n <= 3000`đèn lồng. Một số nguyên không dấu 16 bit có thể biểu diễn các giá trị thông qua`65535`, vì vậy mọi trạng thái DP đều phù hợp một cách thoải mái. Điều này giúp tiết kiệm một lượng lớn bộ nhớ so với các đối tượng số nguyên đầy đủ của Python. 

Số nguyên Python cũng đánh giá một cách an toàn`2 * x[i]`ngay cả khi`x[i]`đang ở gần`10^18`, do đó không có vấn đề tràn. 

Đầu vào có chính xác một ca kiểm thử nên không có vòng lặp ca kiểm thử bên ngoài. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là`1 2 3`. Ban đầu mỗi cặp đều có giá trị DP`2`. Khi cặp`(1, 2)`được xem xét trong các chỉ số dựa trên số không`(0, 1)`, không có tiền thân. Đối với cặp`(2, 3)`, số tiền trước cần thiết là`1`, tồn tại nên độ dài trở thành`dp[0][1] + 1 = 3`. 

|`i`|`j`|`target = 2*x[i]-x[j]`|`k`sau khi tìm kiếm | Tiểu bang | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | -1 |`2`|`2`| 
| 0 | 2 | -1 | -1 |`2`|`2`| 
| 1 | 2 | 1 | 0 |`dp[0][1]+1 = 3`|`3`| 

Câu trả lời cuối cùng là`3`. Phần quan trọng của dấu vết là sự chuyển đổi từ cặp`(1, 2)`ĐẾN`(1, 2, 3)`: tọa độ tiền nhiệm được xây dựng lại theo đại số thay vì quét tất cả các đèn lồng trước đó. 

### Mẫu 2 

Tọa độ là`1 2 4 6 7`. Tiến trình tối ưu là`1, 4, 7`, điểm khác biệt chung của nó là`3`. 

|`i`|`j`|`target`|`k`| Tiểu bang | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | -1 |`2`|`2`| 
| 0 | 2 | -2 | -1 |`2`|`2`| 
| 0 | 3 | -4 | -1 |`2`|`2`| 
| 0 | 4 | -5 | -1 |`2`|`2`| 
| 1 | 2 | 0 | -1 |`2`|`2`| 
| 1 | 3 | -2 | -1 |`2`|`2`| 
| 1 | 4 | -3 | -1 |`2`|`2`| 
| 2 | 3 | 2 | 1 |`dp[1][2]+1 = 3`|`3`| 
| 2 | 4 | 1 | 0 |`dp[0][2]+1 = 3`|`3`| 
| 3 | 4 | 5 | 2 | không có người tiền nhiệm chính xác |`3`| 

Nhà nước cho`(i,j) = (2,4)`tương ứng với tọa độ`4`Và`7`. Người tiền nhiệm bắt buộc của nó là`1`, hiện diện tại chỉ mục`0`. Từ`dp[0][2]`đại diện cho cặp`1,4`, trạng thái mới thể hiện chính xác`1,4,7`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n^2)`| có`O(n^2)`cặp và mỗi con trỏ trước chỉ di chuyển sang trái trên hàng của nó | 
| Không gian |`O(n^2)`| có`n(n-1)/2`trạng thái cặp, được lưu trữ dưới dạng số nguyên 16 bit | 

Vì`n = 3000`, chỉ có khoảng 4,5 triệu trạng thái DP. Với hai byte mỗi trạng thái, bản thân bộ nhớ DP có dung lượng khoảng 9 MB, thấp hơn nhiều so với giới hạn bộ nhớ 512 MB. Số lần chuyển đổi cặp cũng vào khoảng 4,5 triệu, do đó phương pháp bậc hai phù hợp với giới hạn 2 giây đã cho. 

## Trường hợp thử nghiệm```python
import sys
import io
from array import array

def solve():
    input = sys.stdin.readline

    n = int(input())
    x = list(map(int, input().split()))

    if n <= 2:
        print(n)
        return

    rows = [None] * n
    answer = 2

    for i in range(n - 1):
        length = n - i - 1
        row = array('H', [2]) * length

        k = i - 1
        xi = x[i]

        for j in range(i + 1, n):
            target = 2 * xi - x[j]

            while k >= 0 and x[k] > target:
                k -= 1

            if k >= 0 and x[k] == target:
                value = rows[k][i - k - 1] + 1
                row[j - i - 1] = value
                if value > answer:
                    answer = value

        rows[i] = row

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("3\n1 2 3\n") == "3\n", "sample 1"
assert run("5\n1 2 4 6 7\n") == "3\n", "sample 2"
assert run("10\n5 10 15 20 35 60 80 85 110 120\n") == "5\n", "sample 3"

# Minimum-size input with no arithmetic progression of length 3.
assert run("3\n0 1 3\n") == "2\n", "minimum-size non-AP case"

# Boundary coordinates near both ends of the allowed range.
assert run("3\n0 500000000000000000 1000000000000000000\n") == "3\n", "large-coordinate arithmetic progression"

# Long progression with irrelevant gaps around it.
assert run("5\n0 3 6 9 20\n") == "4\n", "extension through several DP states"

# Maximum-size stress case, all gaps equal.
n = 3000
coords = " ".join(map(str, range(n)))
assert run(f"{n}\n{coords}\n") == "3000\n", "maximum-size all-equal-gap case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 0 1 3`|`2`| Đầu vào có kích thước tối thiểu và quy tắc hai đèn lồng tùy ý luôn hợp lệ | 
|`3 / 0 500000000000000000 1000000000000000000`|`3`| Tọa độ lớn và số học số nguyên chính xác | 
|`5 / 0 3 6 9 20`|`4`| Các phần mở rộng DP lặp đi lặp lại và bỏ qua đèn lồng cuối cùng không liên quan | 
|`3000 / 0 1 2 ... 2999`|`3000`| Tối đa`n`, khoảng cách bằng nhau và câu trả lời tối đa có thể | 

## Vỏ cạnh 

cho`0 1 3`, lựa chọn ba chiếc đèn lồng duy nhất có thể có những khoảng trống`1`Và`2`, vậy câu trả lời là`2`. Thuật toán bắt đầu mỗi cặp ở giá trị`2`. Khi nó kiểm tra cặp`(1,3)`, số tiền trước cần thiết là`-1`, nằm ngoài mảng tọa độ nên trạng thái vẫn giữ nguyên`2`. Không có trạng thái nào đạt tới`3`, và câu trả lời cuối cùng vẫn là`2`. 

Vì`0 500000000000000000 1000000000000000000`, tọa độ giữa chính xác là trung bình của các điểm cuối. Khi`i`trỏ đến tọa độ giữa và`j`trỏ đến tọa độ cuối cùng, mục tiêu là`2 * 500000000000000000 - 1000000000000000000 = 0`. Con trỏ tiền nhiệm tìm tọa độ`0`, do đó trạng thái trở thành`3`. Điều này xác nhận rằng phép tính vẫn chính xác ngay cả ở gần ranh giới tọa độ trên. 

Vì`0 3 6 9 20`, sự tiến triển`0,3,6,9`được xây dựng dần dần. Nhà nước cho`(0,3)`có chiều dài`2`. Khi xem xét`(3,6)`, số tiền trước cần thiết là`0`, do đó giá trị của nó trở thành`3`. Cuối cùng,`(6,9)`yêu cầu người tiền nhiệm`3`, trạng thái tương ứng của nó có độ dài`3`, cho chiều dài`4`. Tọa độ cuối cùng`20`không thể kéo dài tiến trình đó, vì vậy câu trả lời là`4`. 

Đối với trường hợp kích thước tối đa`0 1 2 ... 2999`, mỗi cặp thuộc về một cấp số nhân có thể được mở rộng nhiều lần. Ví dụ: các trạng thái kết thúc tại`(0,1)`,`(1,2)`,`(2,3)`, v.v., tất cả đều mở rộng đến các trạng thái dài hơn. Trạng thái cuối cùng đạt chiều dài`3000`. Điều này cũng thực hiện biểu diễn DP 16 bit, vì`3000`ở bên dưới an toàn`65535`. 

Một đầu vào bằng chữ chứa tọa độ bằng nhau, chẳng hạn như`1 1 1`, không phải là phép thử hợp lệ cho vấn đề này vì đầu vào đảm bảo mức tăng nghiêm ngặt. Việc triển khai dựa vào thuộc tính đó khi nó kết luận rằng phần trước bắt buộc của`(i,j)`phải có chỉ số nhỏ hơn`i`. Các tọa độ bằng nhau sẽ tạo ra sự khác biệt bằng 0 và vi phạm mô hình đầu vào đã nêu.
