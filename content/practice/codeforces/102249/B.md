---
title: "CF 102249B - Nhảy vọt: Ch. 2"
description: "Chúng ta có một hàng hoa huệ được biểu thị bằng một chuỗi. Ký tự đầu tiên luôn là A, đại diện cho Alpha Frog, trong khi mọi ký tự sau đó đều là B, đại diện cho Beta Frog hoặc ., đại diện cho một ô trống."
date: "2026-08-17T21:58:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102249
codeforces_index: "B"
codeforces_contest_name: "2019 Facebook Hacker Cup, Qualification Round"
rating: 0
weight: 102249
solve_time_s: 103
verified: true
draft: false
---

[CF 102249B - Nhảy vọt: Ch. 2](https://codeforces.com/problemset/problem/102249/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng hoa huệ được biểu thị bằng một chuỗi. Ký tự đầu tiên luôn là`A`, đại diện cho Alpha Frog, trong khi mọi nhân vật sau này đều là`B`, đại diện cho một con ếch Beta, hoặc`.`, đại diện cho một miếng đệm trống. 

Ếch Alpha bắt đầu từ miếng đệm đầu tiên và muốn đến miếng đệm cuối cùng. Ếch Beta có thể di chuyển một vị trí vào một ô trống liền kề. Ếch Alpha có một động tác khác: nó có thể nhảy qua một khối liên tiếp của một hoặc nhiều Ếch Beta và đáp xuống ô trống đầu tiên ngay sau khối đó. Không giống như chương đầu tiên, Ếch Alpha có thể nhảy theo một trong hai hướng. Chúng ta chỉ cần quyết định xem liệu một số chuỗi hành động hợp tác cuối cùng có thể đưa Alpha lên bảng cuối cùng hay không. 

Đối với mỗi trường hợp thử nghiệm, đầu vào sẽ đưa ra một chuỗi như vậy. Đầu ra là`Y`nếu Alpha Frog có thể chạm tới miếng đệm ngoài cùng bên phải và`N`nếu không thì. 

Độ dài có thể lên tới 5.000 và có thể có tới 500 trường hợp thử nghiệm. Việc mô phỏng các cấu hình là hoàn toàn không thực tế vì số lượng cách sắp xếp có thể tăng theo cấp số nhân theo số lượng miếng đệm. Ngay cả một thuật toán thực hiện khối lượng công việc bậc hai cho mỗi ca kiểm thử cũng tốn kém một cách không cần thiết. Quá trình quét tuyến tính của từng chuỗi dễ dàng đủ nhỏ vì tối đa 2,5 triệu ký tự được kiểm tra trên tất cả các trường hợp thử nghiệm nếu mọi trường hợp đều có độ dài tối đa. 

Có một số trường hợp nhỏ khi thực hiện bất cẩn sẽ đưa ra câu trả lời sai. Vì`A.`, câu trả lời là`N`: Alpha không có Beta Frog để nhảy qua, và việc di chuyển Beta là không thể vì không có. Vì`AB`, câu trả lời cũng là`N`, mặc dù có Beta Frog ngay bên cạnh Alpha, vì không có ô trống nào sau nó. Vì`AB.`, câu trả lời là`Y`: Alpha nhảy qua con ếch Beta duy nhất và đáp xuống tấm đệm cuối cùng. Cuối cùng,`ABB`là`N`: cả hai miếng đệm sau Alpha đều chứa Beta Frogs nên không có bãi đáp. Một lỗi phổ biến là chỉ kiểm tra xem có ít nhất một Beta Frog mà không kiểm tra xem bãi đáp có tồn tại hay không. 

Quy tắc mới cho phép Alpha nhảy theo cả hai hướng tạo ra một trường hợp tinh vi khác. Với hai hoặc nhiều Beta Frogs, câu trả lời có thể là`Y`ngay cả khi không có đủ Beta Frog cho điều kiện nhảy vọt một chiều thông thường. Ví dụ,`A.B..BBB.`có ba con ếch Beta và có thể truy cập được. Xử lý vấn đề này giống hệt như Chương 1 sẽ bác bỏ nó một cách không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp là coi mọi sự sắp xếp hoàn chỉnh của ếch như một trạng thái và thực hiện tìm kiếm theo chiều rộng. Từ một trạng thái, chúng ta có thể liệt kê mọi bước di chuyển Beta hợp pháp và mọi bước nhảy Alpha hợp pháp, sau đó tiếp tục từ mọi trạng thái mới được phát hiện. Điều này đúng vì mọi chuỗi di chuyển hợp pháp của ếch đều tương ứng với một đường dẫn trong biểu đồ trạng thái này, do đó, việc đạt đến trạng thái có Alpha trên ô cuối cùng chính xác là điều kiện mong muốn. 

Vấn đề là số lượng trạng thái. Nếu Alpha có thể chiếm giữ bất kỳ`N`các vị trí và mọi vị trí khác độc lập đều chứa Beta Frog hoặc một ô trống, có thể có`N * 2^(N-1)`các cấu hình riêng biệt. Kiểm tra đến`O(N)`các bước di chuyển có thể có từ mỗi cấu hình sẽ đưa ra số lượng kiểm tra chuyển tiếp trong trường hợp xấu nhất là`O(N^2 * 2^N)`. Vì`N = 5000`, điều này không khả thi chút nào. 

Quan sát hữu ích là vị trí chi tiết của Beta Frogs không quan trọng đối với quyết định cuối cùng. Chỉ có số lượng của họ là quan trọng. Cho phép`n = N - 1`, số lượng miếng đệm sau miếng đệm bắt đầu của Alpha và đặt`b`là số lượng ếch Beta. 

Có ba điều kiện cấu trúc. 

Đầu tiên, nếu`n = 1`, Alpha không bao giờ có thể chạm tới miếng đệm thứ hai. Chỉ có một bệ ở bên phải, hoặc nó trống và Alpha không thể nhảy, hoặc nó chứa Ếch Beta và không có bệ hạ cánh trống. 

Thứ hai, nếu mọi ô sau Alpha đều bị Beta Frog chiếm giữ, thì`b = n`, không có bãi đáp trống ở đâu cả. Alpha không thể di chuyển. 

Phần thú vị là điều gì sẽ xảy ra khi`b < n`, vậy tồn tại ít nhất một ô trống. Chỉ với một Beta Frog, hạn chế nhảy vọt cũ vẫn được áp dụng. Một con ếch Beta có thể đóng vai trò là vật thể Alpha nhảy qua, nhưng sau khi Alpha nhảy qua nó, con ếch Beta đó sẽ ở phía sau Alpha. Không có Beta Frog thứ hai để tạo ra một bước nhảy hữu ích khác. Vì`b = 1`, trường hợp thành công duy nhất là`n = 2`, cụ thể là`AB.`. 

Với ít nhất hai Beta Frog, Chương 2 sẽ thay đổi hoàn toàn tình thế. Hai chú ếch Beta có thể hợp tác với một miếng đệm trống để tạo ra một mô hình cục bộ lặp đi lặp lại cho phép Alpha tiến bộ mà không yêu cầu một chú ếch Beta mới phải di chuyển một vị trí về phía đích. Một phép biến đổi cục bộ đại diện là```
ABB.. -> AB.B. -> .BAB. -> .B.BA
```Ếch Beta đầu tiên di chuyển vào lỗ có sẵn, Alpha sau đó nhảy qua Ếch Beta ngay bên phải của nó và Alpha có thể tiếp tục sử dụng Ếch Beta thứ hai. Ý tưởng tương tự có thể được di chuyển qua hàng. Chỉ cần có ít nhất một ô trống và ít nhất hai con ếch Beta, những con ếch có thể tự sắp xếp lại để cơ chế hai Beta này giúp Alpha di chuyển về đích. Đây là khả năng bổ sung được giới thiệu trong Chương 2. Tiêu chí phù hợp với đặc tính giải pháp đã được thiết lập cho vấn đề. 

Ngưỡng Chương 1 cũng có thể được viết là`b >= ceil(n / 2)`. Trong Chương 2, ngưỡng đó chỉ liên quan đến những trường hợp có ít hơn hai con ếch Beta, bởi vì mọi trường hợp đều có`b >= 2`đã có thể truy cập được miễn là có một bảng trống. Do đó, việc thực hiện thuận tiện là```
n = N - 1
b = number of B characters

if n == 1: N
else if b == n: N
else if b >= ceil(n / 2): Y
else if b >= 2: Y
else: N
```các`ceil(n / 2)`condition xử lý chính xác trường hợp một phiên bản Beta, trong khi`b >= 2`nắm bắt cơ chế Chương 2 mới. Đây cũng là đặc tính nhỏ gọn được sử dụng trong các giải pháp được công bố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(N^2 2^N)`|`O(N 2^N)`| Quá chậm | 
| Tối ưu |`O(N)`|`O(1)`bên cạnh chuỗi đầu vào | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi và để`n = len(s) - 1`. Chúng tôi loại trừ ban đầu`A`bởi vì mọi chuyển động liên quan đều diễn ra giữa các miếng đệm ở bên phải nó. 
2. Đếm số`b`của`B`nhân vật. Vị trí chính xác của những con ếch Beta đó là không cần thiết một khi chúng ta biết các điều kiện cấu trúc ở trên. 
3. Nếu`n == 1`, trả lời ngay`N`. Alpha không thể thực hiện cú nhảy hợp pháp qua một con ếch Beta mà vẫn hạ cánh ở đâu đó. 
4. Nếu`b == n`, trả lời`N`. Mọi bãi đáp sau Alpha đều đã có người sử dụng nên không còn bãi đáp trống nào. 
5. Tính toán`ceil(n / 2)`BẰNG`(n + 1) // 2`. Nếu như`b`ít nhất là giá trị này, hãy trả lời`Y`. Điều này bao gồm sự sắp xếp bước nhảy vọt tiêu chuẩn, bao gồm cả trường hợp hữu ích duy nhất với một con ếch Beta. 
6. Nếu điều kiện trước đó không thành công nhưng`b >= 2`, trả lời`Y`. Sự hiện diện của hai con ếch Beta sẽ kích hoạt cơ chế Chương 2 mới, miễn là có một miếng đệm trống. các`b == n`trường hợp đã bị từ chối nên có ít nhất một lỗ trống. 
7. Nếu không có trường hợp nào trước đó áp dụng, hãy trả lời`N`. Điều này có nghĩa là Alpha có quá ít Beta Frog để đạt được tiến bộ và cơ chế hai Beta đặc biệt không khả dụng. 

Tại sao nó hoạt động: thuật toán phân tách chính xác các cấu hình ngăn chặn mọi chuyển động khỏi những cấu hình có cơ chế hợp tác hợp lệ. các`n == 1`trường hợp không có khả năng nhảy Alpha. các`b == n`trường hợp không có bãi đáp có thể. Khi có ít hơn hai con ếch Beta, yêu cầu nhảy vọt thông thường sẽ xác định liệu Alpha có đủ hỗ trợ để đi đến cuối hay không. Khi tồn tại hai con ếch Beta và ít nhất một miếng đệm trống, chuyển động của chúng có thể tạo ra mô hình hai-Beta của Chương 2 và đưa Alpha tiến về phía trước. Do đó, mọi trường hợp được chấp nhận đều có một chuỗi các bước đi hợp lệ, trong khi mọi trường hợp bị bác bỏ đều thiếu cấu trúc cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(s):
    n = len(s) - 1
    b = s.count('B')

    if n == 1:
        return 'N'

    if b == n:
        return 'N'

    if b >= (n + 1) // 2:
        return 'Y'

    if b >= 2:
        return 'Y'

    return 'N'

def main():
    t = int(input())
    out = []

    for case_id in range(1, t + 1):
        s = input().strip()
        answer = solve_case(s)
        out.append(f"Case #{case_id}: {answer}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```các`solve_case`đầu tiên hàm chuyển đổi độ dài chuỗi thành`n`, số miếng đệm ở bên phải của Alpha. Điều này khớp trực tiếp với đặc tính toán học và tránh trộn lẫn bảng Alpha ban đầu vào các điều kiện đếm ếch.`str.count('B')`cung cấp thông tin duy nhất về chuỗi mà quyết định cần. Không cần phải sửa đổi cách sắp xếp hoặc mô phỏng các bước di chuyển của Beta Frog. 

điều kiện`b == n`phải được kiểm tra trước các điều kiện tích cực. Ví dụ,`ABB`có`n = 2`Và`b = 2`, do đó biểu thức`b >= (n + 1) // 2`là đúng. Tuy nhiên câu trả lời là`N`, vì Alpha không có nơi nào để hạ cánh. Đảo ngược các kiểm tra này sẽ tạo ra một câu trả lời sai. 

biểu thức`(n + 1) // 2`tính toán trần của`n / 2`sử dụng số học số nguyên. Số nguyên Python không có vấn đề tràn ở đây và tất cả các giá trị đều rất nhỏ so với phạm vi số nguyên của ngôn ngữ. 

Vòng lặp bên ngoài thêm yêu cầu`Case #i: `tiền tố và thu thập các câu trả lời trước khi viết chúng một lần. Điều này giúp I/O đơn giản trong khi vẫn xử lý thoải mái tất cả 500 trường hợp kiểm thử. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`A.`. 

| Biến | Giá trị | 
| --- | --- | 
|`s`|`A.`| 
|`n`|`1`| 
|`b`|`0`| 
|`n == 1`| đúng | 
| Trả lời |`N`| 

Điều kiện đầu tiên ngay lập tức bác bỏ trường hợp này. Chỉ có một miếng đệm ở bên phải của Alpha, vì vậy không thể thực hiện bước nhảy Alpha hợp pháp. 

Đối với Mẫu 2, đầu vào là`AB.`. 

| Biến | Giá trị | 
| --- | --- | 
|`s`|`AB.`| 
|`n`|`2`| 
|`b`|`1`| 
|`n == 1`| sai | 
|`b == n`| sai | 
|`b >= ceil(n / 2)`|`1 >= 1`, đúng | 
| Trả lời |`Y`| 

Con ếch Beta duy nhất nằm ở phần giữa và phần cuối cùng trống. Alpha có thể nhảy qua Beta Frog và hạ cánh trực tiếp trên tấm đệm cuối cùng. 

Đối với một ví dụ cụ thể của Chương 2, hãy xem xét`A.B..BBB.`. 

| Biến | Giá trị | 
| --- | --- | 
|`s`|`A.B..BBB.`| 
|`n`|`8`| 
|`b`|`4`| 
|`n == 1`| sai | 
|`b == n`| sai | 
|`b >= ceil(n / 2)`|`4 >= 4`, đúng | 
| Trả lời |`Y`| 

Ở đây ví dụ cũng thỏa mãn ngưỡng thông thường. Một trường hợp rõ ràng hơn là`A.B..BBB.`với ba con ếch Beta sau khi điều chỉnh hàng thành`A.B..BB..`, Ở đâu`n = 8`Và`b = 3`. Ngưỡng Chương 1 thông thường sẽ yêu cầu bốn Ếch Beta, nhưng Chương 2 chấp nhận vì`b >= 2`và có một miếng đệm trống. Đây chính xác là sức mạnh bổ sung được cung cấp bằng cách cho phép Alpha di chuyển theo một trong hai hướng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N)`mỗi trường hợp thử nghiệm | Đếm`B`ký tự quét chuỗi một lần. | 
| Không gian |`O(1)`phụ trợ | Chỉ cần chuỗi, độ dài và số lượng Beta. | 

Với`N <= 5000`và tối đa 500 trường hợp thử nghiệm, ngay cả đầu vào lý thuyết tối đa cũng chỉ chứa khoảng 2,5 triệu ký tự. Một lần quét tuyến tính duy nhất cho mỗi trường hợp là thoải mái trong phạm vi đó. Thuật toán không xây dựng trạng thái, thực hiện đệ quy hoặc phân bổ các cấu trúc tỷ lệ thuận với số cách sắp xếp ếch có thể có. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(s):
    n = len(s) - 1
    b = s.count('B')

    if n == 1:
        return 'N'
    if b == n:
        return 'N'
    if b >= (n + 1) // 2:
        return 'Y'
    if b >= 2:
        return 'Y'
    return 'N'

def solve_input(inp: str) -> str:
    data = inp.strip().splitlines()
    t = int(data[0])
    ans = []

    for case_id in range(1, t + 1):
        ans.append(f"Case #{case_id}: {solve_case(data[case_id])}")

    return "\n".join(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    t = int(input())
    out = []

    for case_id in range(1, t + 1):
        s = input().strip()
        out.append(f"Case #{case_id}: {solve_case(s)}")

    print("\n".join(out))

    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

sample_input = """8
A.
AB.
ABB
A.BB
A..BB..B
A.B..BBB.
AB.........
A.B..BBBB.BB
"""

sample_output = """Case #1: N
Case #2: Y
Case #3: N
Case #4: Y
Case #5: Y
Case #6: Y
Case #7: N
Case #8: Y"""

assert run(sample_input) == sample_output, "provided samples"

assert run("""2
A.
AB
""") == """Case #1: N
Case #2: N""", "minimum-size cases"

assert run("""4
AB.
A.B
A.B.
A.BB
""") == """Case #1: Y
Case #2: Y
Case #3: N
Case #4: Y""", "boundary cases"

assert run("""3
A
""" .strip() + "\n" if False else """3
A.
A..
A...
""") == """Case #1: N
Case #2: N
Case #3: N""", "no Beta Frogs"

assert run("""2
ABBB
A.BB
""") == """Case #1: N
Case #2: Y""", "all occupied versus two-Beta mechanism"

assert run("2\nA" + "B" * 4999 + "\n" + "A" + "BB" + "." * 4997 + "\n") == \
       "Case #1: N\nCase #2: Y", "maximum-size cases"
```Các trường hợp kích thước tối thiểu kiểm tra cả hai chuỗi có thể có độ dài bằng hai. Cả hai đều không thể di chuyển Alpha đến phần đệm cuối cùng, điều này sẽ thu hút sự chú ý đặc biệt.`n == 1`tình trạng. 

Các trường hợp ranh giới phân biệt`AB.`từ`A.B.`. Con trước có một con ếch Beta và có đủ miếng đệm để nhảy thành công, trong khi con sau có một con ếch Beta nhưng có quá nhiều miếng đệm nên phải từ chối.`A.BB`kiểm tra xem hai con ếch Beta có đủ không ngay cả khi ngưỡng thông thường không phải là lý do quyết định. 

Các trường hợp không có Beta xác minh rằng một hàng trống bằng cách nào đó không thể được coi là một nước đi Alpha hợp lệ. Trường hợp chiếm tất cả`ABBB`xác minh rằng`b == n`kiểm tra được ưu tiên hơn các điều kiện đếm dương. 

Các thử nghiệm kích thước tối đa kiểm tra cả hậu tố bị chiếm dụng hoàn toàn và một chuỗi lớn chỉ chứa hai con ếch Beta. Họ cũng xác minh rằng việc triển khai vẫn tuyến tính khi`N = 5000`. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A.`Và`AB`|`N`,`N`| Ranh giới kích thước tối thiểu | 
|`AB.`,`A.B`,`A.B.`,`A.BB`|`Y`,`Y`,`N`,`Y`| Ngưỡng một Beta và quy tắc hai Beta | 
|`A.`,`A..`,`A...`|`N`,`N`,`N`| Không có ếch Beta | 
|`ABBB`,`A.BB`|`N`,`Y`| Không có bệ hạ cánh so với cơ chế Chương 2 | 
|`A`+ 4999`B`|`N`| Kích thước tối đa và hậu tố bị chiếm dụng hoàn toàn | 
|`A`+`BB`+ 4997`.`|`Y`| Kích thước tối đa với đúng hai con ếch Beta | 

## Vỏ cạnh 

Đầu vào nhỏ nhất có thể là`A.`. Đây`n = 1`Và`b = 0`, do đó thuật toán trả về`N`trước khi xem xét bất kỳ điều kiện nào khác. Điều này tránh việc xử lý không chính xác vùng đệm thứ hai trống như một đích đến có thể đến được bằng cách di chuyển một bước thông thường. 

Vì`AB`, chúng tôi có`n = 1`Và`b = 1`. Thuật toán vẫn trả về`N`từ điều kiện đầu tiên. Mặc dù Alpha có một con ếch Beta để nhảy qua nhưng không có bãi trống nào sau con ếch Beta đó nên cú nhảy không có vị trí hạ cánh hợp pháp. 

Vì`AB.`, chúng tôi có`n = 2`Và`b = 1`. Điều kiện đầu tiên không thành công, hậu tố không được chiếm hoàn toàn và`b >= ceil(2 / 2)`là đúng. Thuật toán trả về`Y`, phù hợp với việc di chuyển trực tiếp từ`A`qua`B`vào chung kết`.`. 

Vì`ABB`, chúng tôi có`n = 2`Và`b = 2`. Kiểm tra ngưỡng sẽ có vẻ chấp nhận trường hợp này, vì`2 >= 1`, nhưng thuật toán kiểm tra`b == n`đầu tiên và trở lại`N`. Mọi bãi đáp sau Alpha đều đã bị chiếm đóng nên không còn bãi đáp nào nữa. Thứ tự này là cần thiết. 

Vì`A.B.`, chúng tôi có`n = 3`Và`b = 1`. Một con ếch Beta đơn lẻ không đủ cho một cuộc hành trình dài thế này. Ngưỡng trần là`2`, Vì thế`b = 1`thất bại, và`b >= 2`cũng thất bại. Câu trả lời là`N`. 

Đối với trường hợp như`A.BB`, chúng tôi có`n = 3`Và`b = 2`. Có một miếng đệm trống, vì vậy`b != n`và hai con ếch Beta kích hoạt cơ chế Chương 2. Thuật toán trả về`Y`. Giải pháp của Chương 1 chỉ dựa trên mô hình chuyển động cũ có thể xử lý sai quy tắc rộng hơn này. 

Cuối cùng, đối với chuỗi có kích thước tối đa bao gồm`A`theo sau là 4.999`B`nhân vật,`n = 4,999`Và`b = 4,999`. Thuật toán trả về`N`ngay lập tức vì toàn bộ hậu tố đã bị chiếm dụng. Đối với một chuỗi kích thước tối đa bao gồm`A`, hai`B`ký tự và 4.997 ô trống,`b = 2 < n`, vậy câu trả lời là`Y`. Hai trường hợp này cho thấy tại sao phải xem xét cả số lượng Beta Frog và sự tồn tại của ít nhất một bãi đáp trống.
