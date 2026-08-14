---
title: "CF 102302A - Tòa nhà nhảy"
description: "Chúng ta có một dãy gồm (N) tòa nhà, trong đó tòa nhà (i) có chiều cao (hi). Lario bắt đầu trên một tòa nhà đã chọn và thực hiện đúng một bước nhảy."
date: "2026-08-13T07:33:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102302
codeforces_index: "A"
codeforces_contest_name: "2019 USP-ICMC"
rating: 0
weight: 102302
solve_time_s: 149
verified: true
draft: false
---

[CF 102302A - Tòa nhà nhảy](https://codeforces.com/problemset/problem/102302/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy gồm (N) tòa nhà, trong đó tòa nhà (i) có chiều cao (h_i). Lario bắt đầu trên một tòa nhà đã chọn và thực hiện đúng một bước nhảy. Nếu anh ta bắt đầu tại tòa nhà (i), cú nhảy có thể bao gồm hầu hết các vị trí (h_i) ở bên phải, do đó, đích đến của anh ta sẽ là 

[ 
\min(i+h_i,N) 
] 

sử dụng chỉ mục dựa trên 1. 

Trong khi di chuyển qua khoảng thời gian đó, Lario đâm vào tòa nhà đầu tiên có chiều cao lớn hơn chiều cao của tòa nhà ban đầu của anh ấy. Nếu tòa nhà cao hơn đó ở vị trí (j), anh ta không thể với tới nó và đáp xuống (j-1). Đầu ra được yêu cầu là quãng đường đã di chuyển chứ không phải chỉ số của điểm đến. Vì vậy, nếu không có tòa nhà nào cao hơn trước điểm đến dự kiến ​​thì câu trả lời là điểm đến dự định trừ đi (i). Nếu tòa nhà cao hơn đầu tiên ở (j), câu trả lời là (j-i-1). 

Ví dụ: với độ cao (5,2,2,3,6,2), bắt đầu từ tòa nhà 1 sẽ cho điểm đến tối đa là tòa nhà 6. Tòa nhà 5 có chiều cao 6, cao hơn chiều cao bắt đầu 5, do đó Lario đáp xuống tòa nhà 4. Khoảng cách là (4-1=3), là giá trị đầu tiên của đầu ra mẫu. 

Ràng buộc (N\le 10^5) loại trừ các thuật toán quét liên tục một phần lớn của mảng cho mọi vị trí bắt đầu. Một thuật toán bậc hai có thể thực hiện khoảng (N(N-1)/2), gần đúng (5\cdot10^9), so sánh trong trường hợp xấu nhất khi (N=10^5). Điều đó vượt xa những gì giới hạn 2 giây có thể đáp ứng. Chúng ta cần một giải pháp gần với thời gian tuyến tính. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai có vẻ hợp lý trở nên không chính xác. Đầu tiên là chiều cao bằng nhau. Các tòa nhà bằng nhau không gây ra va chạm vì chướng ngại vật phải cao hơn. Đối với đầu vào`2 2 2`, đầu ra đúng là`2 1 0`. Một tìm kiếm sử dụng`>=`thay vì`>`sẽ dừng sai ở tòa nhà tiếp theo. 

Trường hợp thứ hai là khi tòa nhà cao hơn chính xác là điểm đến dự kiến. Đối với đầu vào```
2
1 2
```tòa nhà đầu tiên có thể cố gắng tiếp cận tòa nhà 2, nhưng tòa nhà 2 cao hơn, vì vậy Lario đáp xuống tòa nhà 1. Kết quả đầu ra đúng là`0 0`. Việc triển khai bất cẩn chỉ tìm kiếm các vị trí nghiêm ngặt trước đích sẽ báo cáo không chính xác`1`cho tòa nhà đầu tiên. 

Trường hợp thứ ba xảy ra khi có một tòa nhà cao hơn nhưng lại nằm ngoài phạm vi nhảy. Đối với đầu vào```
4
2 1 1 3
```bắt đầu từ tòa nhà 1 chỉ cho phép nhảy xa đến tòa nhà 3. Phải bỏ qua tòa nhà cao hơn ở vị trí 4, vì vậy câu trả lời đầu tiên là`2`. Đầu ra đúng là`2 1 0 0`. Việc tìm kiếm tòa nhà cao hơn tiếp theo mà không kiểm tra xem liệu nó có nằm trong phạm vi cho phép hay không sẽ cho kết quả sai. 

Trường hợp ranh giới cuối cùng là một tòa nhà duy nhất. Vì```
1
7
```không có nơi nào để di chuyển, vì vậy đầu ra là`0`. Đây cũng là một cách kiểm tra hữu ích để đảm bảo tính toán đích và tra cứu ngăn xếp xử lý chính xác vị trí cuối cùng. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là xử lý mọi vị trí bắt đầu một cách độc lập. Đối với tòa nhà (i), trước tiên hãy tính vị trí xa nhất mà bước nhảy có thể đạt tới. Sau đó kiểm tra các tòa nhà (i+1,i+2,\ldots) theo thứ tự cho đến khi tìm thấy một tòa nhà cao hơn hoặc đạt đến giới hạn nhảy. Nếu tòa nhà cao hơn đầu tiên ở (j), hãy quay lại (j-i-1). Nếu không thì trả lại toàn bộ khoảng cách nhảy có thể. 

Phương pháp bạo lực này là đúng vì trò chơi chỉ phụ thuộc vào tòa nhà cao hơn đầu tiên gặp dọc theo đường nhảy. Quét từ trái sang phải sẽ tìm thấy chính xác tòa nhà đó và dừng ở giới hạn bước nhảy một cách chính xác sẽ bỏ qua các chướng ngại vật không thể tiếp cận trong lần nhảy này. 

Vấn đề là số lượng công việc lặp đi lặp lại. Hãy xem xét (N=10^5) tòa nhà có chiều cao bằng nhau và phạm vi nhảy đủ lớn. Không có xung đột nào xảy ra, do đó quá trình quét tòa nhà đầu tiên sẽ kiểm tra gần như toàn bộ hậu tố, quá trình quét tòa nhà thứ hai sẽ kiểm tra gần như toàn bộ hậu tố còn lại, v.v. Số lượng vị trí được kiểm tra có thể đạt tới 

[ 
1+2+\cdots +(N-1)=\frac{N(N-1)}2=4.999.950.000. 
] 

Lực lượng vũ phu có tác dụng vì mọi câu trả lời đều được xác định bởi tòa nhà cao hơn đầu tiên, nhưng nó không thành công khi nhiều vị trí xuất phát về cơ bản hỏi cùng một câu hỏi về các hậu tố chồng chéo. 

Quan sát quan trọng là đối với mỗi tòa nhà, chúng ta thực sự không cần phải tìm kiếm toàn bộ khoảng thời gian nhảy của nó. Chúng tôi chỉ cần tòa nhà gần bên phải nhất và cao hơn. Gọi vị trí này là phần tử lớn hơn tiếp theo. Nếu tòa nhà cao hơn gần nhất đã vượt quá giới hạn nhảy, thì mọi tòa nhà cao hơn khác thậm chí còn ở xa hơn, do đó không có va chạm bên trong bước nhảy. Nếu nằm trong giới hạn thì đó tự động là lần va chạm đầu tiên. 

Điều này chuyển đổi vấn đề thành một phép tính phần tử lớn hơn tiếp theo tiêu chuẩn. Một ngăn xếp đơn điệu có thể tìm thấy tòa nhà lớn hơn gần nhất cho mọi vị trí trong thời gian (O(N)). Chúng tôi xử lý mảng từ phải sang trái. Ngăn xếp chứa các vị trí ứng cử viên ở bên phải có chiều cao chưa bị tòa nhà gần hơn loại bỏ. 

Giả sử chúng ta đang xử lý việc xây dựng (i). Bất kỳ vị trí ngăn xếp nào có chiều cao nhỏ hơn hoặc bằng (h_i) đều không thể là tòa nhà lớn hơn tiếp theo cho (i), vì bản thân tòa nhà (i) ít nhất phải cao bằng và gần với bất kỳ vị trí nào xa hơn bên trái. Những vị trí như vậy được bật lên. Sau khi tất cả các vị trí như vậy được loại bỏ, đỉnh của ngăn xếp, nếu có, là vị trí gần bên phải nhất với chiều cao lớn hơn (h_i). 

Sự so sánh là`<=`, không`<`, vì chiều cao bằng nhau không phải là va chạm. Đây là chi tiết làm cho ngăn xếp thể hiện những tòa nhà cao hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2)) | (O(1)) phụ trợ | Quá chậm | 
| Tối ưu | (O(N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng tòa nhà và chiều cao của chúng. Chúng tôi sử dụng các chỉ số dựa trên 0 trong nội bộ vì mảng Python sử dụng chúng một cách tự nhiên, trong khi khoảng cách cuối cùng có thể được tính trực tiếp từ sự khác biệt về chỉ số. 
2. Tạo một mảng`next_greater`Ở đâu`next_greater[i]`sẽ chứa chỉ mục của tòa nhà gần nhất bên phải có chiều cao lớn hơn`h[i]`. Sử dụng`-1`khi không có tòa nhà như vậy tồn tại. 
3. Quét các tòa nhà từ phải sang trái trong khi duy trì một chồng chỉ số ứng cử viên. Trước khi xử lý vị trí`i`, liên tục xóa đỉnh ngăn xếp khi chiều cao của nó nhỏ hơn hoặc bằng`h[i]`. 

Một tòa nhà bị dỡ bỏ không bao giờ có thể là câu trả lời cho vị trí`i`. Nếu chiều cao của nó nhỏ hơn thì nó không thể là chướng ngại vật. Nếu chiều cao của nó bằng nhau thì nó cũng không thể là chướng ngại vật vì chỉ những tòa nhà cao hơn mới gây ra va chạm. Nó cũng không thể cần thiết như một ứng cử viên ở xa hơn về bên trái sau`i`đã được xử lý vì`i`gần hơn và ít nhất là cao bằng. 
4. Sau bước bật lên, đỉnh ngăn xếp là tòa nhà cao hơn hoàn toàn gần nhất ở bên phải, nếu ngăn xếp không trống. Lưu trữ chỉ mục đó trong`next_greater[i]`, sau đó đẩy`i`vào ngăn xếp để nó có thể trở thành ứng cử viên cho các vị trí còn lại. 
5. Đối với mọi vị trí xuất phát`i`, tính toán tòa nhà xa nhất mà cú nhảy có thể tới được mà không xét đến va chạm. Với việc lập chỉ mục dựa trên số không, đây là 

[ 
target=\min(i+h_i,N-1). 
] 

Quãng đường lớn nhất không va chạm là`target - i`. 
6. Nhìn vào`j = next_greater[i]`. Nếu như`j != -1`Và`j <= target`, thì tòa nhà cao hơn đầu tiên nằm bên trong phạm vi nhảy. Lario tấn công tòa nhà`j`và đáp xuống`j-1`, vậy câu trả lời là 

[ 
j-i-1. 
] 
7. Nếu không có tòa nhà lớn hơn tiếp theo hoặc tòa nhà lớn hơn tiếp theo nằm ngoài`target`, không có va chạm xảy ra trong quá trình nhảy này. Câu trả lời đơn giản là`target - i`. 

### Tại sao nó hoạt động 

Đối với mọi vị trí (i), tính toán ngăn xếp sẽ đưa ra vị trí gần nhất (j>i) với (h_j>h_i) hoặc báo cáo không tồn tại. Đây chính xác là va chạm đầu tiên có thể xảy ra theo hướng vô hạn về bên phải. Nếu (j) nằm trong phạm vi nhảy cho phép, thì không có tòa nhà cao hơn nào có thể xuất hiện trước (j), theo định nghĩa là cao hơn gần nhất, do đó việc hạ cánh ở (j-1) là đúng. Nếu (j) nằm ngoài phạm vi cho phép thì mọi tòa nhà cao hơn cũng nằm ngoài phạm vi đó, do đó cú nhảy sẽ đến đích như dự kiến. Như vậy mọi đáp án đều được xác định chính xác. 

Bản thân ngăn xếp là tuyến tính vì mỗi chỉ mục được đẩy chính xác một lần và xuất hiện nhiều nhất một lần. Thuật toán không bao giờ cần phải xem lại một ứng cử viên đã bị loại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    h = list(map(int, input().split()))

    next_greater = [-1] * n
    stack = []

    for i in range(n - 1, -1, -1):
        while stack and h[stack[-1]] <= h[i]:
            stack.pop()

        if stack:
            next_greater[i] = stack[-1]

        stack.append(i)

    ans = [0] * n

    for i in range(n):
        target = min(i + h[i], n - 1)
        j = next_greater[i]

        if j != -1 and j <= target:
            ans[i] = j - i - 1
        else:
            ans[i] = target - i

    print(*ans)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên xây dựng mảng lớn hơn tiếp theo. Việc xử lý từ phải sang trái là điều làm cho ngăn xếp trở nên hữu ích, bởi vì mọi trở ngại có thể xảy ra đối với`i`đã được xem xét khi`i`đã đạt được. 

điều kiện`h[stack[-1]] <= h[i]`loại bỏ cả các tòa nhà ngắn hơn và bằng nhau. Chỉ sử dụng`<`sẽ coi những độ cao bằng nhau là chướng ngại vật một cách không chính xác. 

Điểm đến sử dụng`n - 1`bởi vì việc thực hiện dựa trên số không. Vấn đề ban đầu là`n`do đó tòa nhà tương ứng với chỉ số`n - 1`. 

Điều kiện va chạm là`j <= target`, không`j < target`. Tòa nhà cao hơn ở đúng vị trí dự định vẫn gây ra va chạm. Trong trường hợp đó`j - i - 1`trở thành chính xác khoảng cách đến tòa nhà ngay trước chướng ngại vật. 

Không có vấn đề tràn số nguyên trong Python. Biểu thức chỉ mục có liên quan lớn nhất theo thứ tự (10^5) và số nguyên Python xử lý trực tiếp. 

trận chung kết`print(*ans)`tạo ra đầu ra được phân tách bằng không gian cần thiết. Sự cố chỉ chứa một trường hợp kiểm thử duy nhất, do đó không cần vòng lặp trường hợp kiểm thử nào. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, đầu vào là```
6
5 2 2 3 6 2
```Các vị trí lớn hơn tiếp theo được tính toán đầu tiên. Bảng sử dụng các chỉ số dựa trên số 0, vì vậy vị trí 0 là tòa nhà đầu tiên. 

| tôi | h[i] | Xếp chồng sau khi bật | next_Greater[i] | 
| --- | --- | --- | --- | 
| 5 | 2 | trống | -1 | 
| 4 | 6 | trống | -1 | 
| 3 | 3 | [4] | 4 | 
| 2 | 2 | [4, 3] | 3 | 
| 1 | 2 | [4, 3] | 3 | 
| 0 | 5 | [4] | 4 | 

Đối với vị trí 0, giới hạn nhảy cho`target = 5`. Tòa nhà cao hơn tiếp theo của nó là vị trí 4, nằm trong phạm vi, vì vậy câu trả lời là`4 - 0 - 1 = 3`. 

Đối với vị trí 1, mục tiêu là vị trí 3. Tòa nhà cao hơn tiếp theo chính xác là vị trí 3, vì vậy câu trả lời là`3 - 1 - 1 = 1`. 

Đối với vị trí 2, tòa nhà cao hơn tiếp theo là vị trí 3 nhưng liền kề. Lario lùi về vị trí số 2, tạo khoảng cách`0`. 

Đối với vị trí 3, tòa nhà cao hơn tiếp theo là vị trí 4. Chiều cao của nó là 6, trong khi chiều cao ban đầu là 3 nên va chạm xảy ra ngay lập tức và đáp án là`0`. 

Đối với vị trí 4, không có tòa nhà nào cao hơn ở bên phải. Chiều cao của nó là 6 và cú nhảy của nó tới tòa nhà cuối cùng, tạo khoảng cách`1`. 

Đối với vị trí 5, bước nhảy không còn nơi nào để đi, mang lại`0`. 

Kết quả đầu ra là```
3 1 0 0 1 0
```Ví dụ này thể hiện cả hai trường hợp va chạm và thực tế là đầu ra biểu thị khoảng cách chứ không phải chỉ số tòa nhà cuối cùng. 

Đối với ví dụ thứ hai, hãy xem xét```
5
2 1 3 1 1
```Phép tính lớn tiếp theo là: 

| tôi | h[i] | Xếp chồng sau khi bật | next_Greater[i] | 
| --- | --- | --- | --- | 
| 4 | 1 | trống | -1 | 
| 3 | 1 | trống | -1 | 
| 2 | 3 | trống | -1 | 
| 1 | 1 | [2] | 2 | 
| 0 | 2 | [2] | 2 | 

Bây giờ hãy đánh giá các bước nhảy: 

| tôi | h[i] | mục tiêu | tiếp theo lớn hơn | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 2 | 1 | 
| 1 | 1 | 2 | 2 | 0 | 
| 2 | 3 | 4 | -1 | 2 | 
| 3 | 1 | 4 | -1 | 1 | 
| 4 | 1 | 4 | -1 | 0 | 

Xuất phát từ vị trí 0, đích đến dự kiến là vị trí 2, nhưng vị trí 2 có độ cao 3, cao hơn 2. Lario đáp xuống vị trí 1 nên khoảng cách là 1. 

Bắt đầu từ vị trí 1, tòa nhà cao hơn lại là vị trí 2, nhưng bây giờ nó nằm liền kề. Lario đáp xuống vị trí 1, tạo khoảng cách 0. 

Đầu ra là```
1 0 2 1 0
```Ví dụ này cho thấy tại sao tòa nhà lớn hơn tiếp theo chỉ quan trọng nếu nó nằm trong giới hạn nhảy riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) | Mỗi tòa nhà được đẩy và bật ra khỏi ngăn xếp đơn điệu nhiều nhất một lần, sau đó là một lượt tuyến tính để tính toán các câu trả lời. | 
| Không gian | (O(N)) | Mảng lớn hơn tiếp theo, mảng trả lời và ngăn xếp, mỗi mảng chứa tối đa (N) phần tử. | 

Với (N=10^5), thuật toán chỉ thực hiện một số lượng nhỏ các hoạt động không đổi trên mỗi tòa nhà. Việc sử dụng bộ nhớ cũng tuyến tính và phù hợp thoải mái với giới hạn 64 MB cho việc triển khai này. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây có thể được đặt trong cùng một tệp với giải pháp hoặc được điều chỉnh thành một khai thác thử nghiệm riêng biệt. Người trợ giúp đặt lại`input`sau khi thay thế đầu vào tiêu chuẩn để có thể kiểm tra chức năng giải pháp nhiều lần.```python
import sys
import io

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdout = old_stdout
        sys.stdin = old_stdin
        input = old_input

# Provided sample
assert run("6\n5 2 2 3 6 2\n") == "3 1 0 0 1 0", "sample 1"

# Minimum-size input
assert run("1\n7\n") == "0", "single building"

# All equal heights, so no building is ever taller
assert run("5\n3 3 3 3 3\n") == "4 3 2 1 0", "all equal"

# A taller building exactly at the jump destination
assert run("2\n1 2\n") == "0 0", "collision at destination"

# A taller building exists, but lies beyond the jump range
assert run("4\n2 1 1 3\n") == "2 1 0 0", "taller building beyond range"

# Maximum-size input
n = 100000
heights = [1] * n
inp = str(n) + "\n" + " ".join(map(str, heights)) + "\n"
expected = " ".join(["1"] * (n - 1) + ["0"])
assert run(inp) == expected, "maximum-size all-equal case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`0`| Kích thước tối thiểu và xử lý vị trí cuối cùng | 
|`5 / 3 3 3 3 3`|`4 3 2 1 0`| Chiều cao bằng nhau không được coi là cao hơn | 
|`2 / 1 2`|`0 0`| Tòa nhà cao hơn ở đúng điểm đến gây ra va chạm | 
|`4 / 2 1 1 3`|`2 1 0 0`| Các tòa nhà cao hơn ngoài phạm vi nhảy phải được bỏ qua | 
| (100000) bản sao của`1`|`1`lặp lại 99999 lần sau đó`0`| Hiệu suất tuyến tính và kích thước đầu vào tối đa | 

## Vỏ cạnh 

Trường hợp xây dựng duy nhất là```
1
7
```Ngăn xếp ban đầu không chứa gì khi vị trí 0 được xử lý, vì vậy`next_greater[0]`là`-1`. Mục tiêu là`min(0+7,0)=0`, cho`target-i=0`. Đầu ra là`0`. 

Để có chiều cao bằng nhau, hãy xem xét```
3
2 2 2
```Ở vị trí thứ 2 ngăn xếp trống. Tại vị trí 1, tòa nhà ở vị trí 2 có chiều cao 2, bằng chiều cao hiện tại nên`<=`điều kiện bật nó lên. Do đó, Vị trí 1 không có tòa nhà lớn hơn tiếp theo. Điều tương tự cũng xảy ra với vị trí 0. Khoảng cách nhảy là 2, 1 và 0, tạo ra```
2 1 0
```Điều này khẳng định rằng sự bình đẳng không bao giờ được coi là một sự va chạm. 

Để có tòa nhà cao hơn chính xác tại điểm đến, hãy sử dụng```
2
1 2
```Đối với vị trí 0, mục tiêu là vị trí 1 và vị trí lớn hơn tiếp theo cũng là 1. Vì`j <= target`, xảy ra va chạm. Lario hạ cánh tại`j-1=0`, vậy câu trả lời là`1-0-1=0`. Vị trí 1 đã là tòa nhà cuối cùng nên đáp án của nó cũng là 0. Kết quả đầu ra là```
0 0
```Điều kiện biên được xử lý bằng phép so sánh không chặt chẽ với`target`. 

Đối với chướng ngại vật nằm ngoài phạm vi cho phép, hãy sử dụng```
4
2 1 1 3
```Tại vị trí 0, tòa nhà lớn hơn tiếp theo là vị trí 3, nhưng mục tiêu của nó chỉ là vị trí 2 vì chiều cao bắt đầu là 2. Vì`3 > 2`, chướng ngại vật không thể cản trở bước nhảy này. Thuật toán trả về`2-0=2`. Ở vị trí 2, độ cao bắt đầu là 1 và mục tiêu là vị trí 3, do đó tòa nhà cao hơn ở vị trí 3 nằm trong phạm vi và câu trả lời trở thành`3-2-1=0`. Đầu ra cuối cùng là```
2 1 0 0
```Điều này chứng tỏ tại sao chỉ tính toán vị trí lớn hơn tiếp theo là không đủ. Giới hạn nhảy vẫn phải được kiểm tra trước khi coi tòa nhà đó là chướng ngại vật.
