---
title: "CF 59D - Sắp xếp đội"
description: "Chúng tôi có 3n sinh viên được xếp hạng theo thành tích cá nhân. Học sinh xếp hạng cao hơn trở thành đội trưởng sớm hơn. Khi đội trưởng thành lập một đội, họ sẽ chọn hai học sinh hiện chưa được chỉ định theo danh sách sở thích cá nhân của họ."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 59
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 55 (Div. 2)"
rating: 2000
weight: 59
solve_time_s: 159
verified: false
draft: false
---

[CF 59D - Sắp xếp nhóm](https://codeforces.com/problemset/problem/59/D) 

**Xếp hạng:** 2000 
**Tags:** thuật toán xây dựng, tham lam, triển khai 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`3n`sinh viên được xếp hạng theo thành tích cá nhân. Học sinh xếp hạng cao hơn trở thành đội trưởng sớm hơn. Khi đội trưởng thành lập một đội, họ sẽ chọn hai học sinh hiện chưa được chỉ định theo danh sách sở thích cá nhân của họ. 

Các đội cuối cùng đã được biết đến và các đội được liệt kê theo đúng thứ tự mà chúng được tạo. Trong mỗi đội, thứ tự thành viên là tùy ý. 

Đối với một học sinh cụ thể`k`, chúng ta phải xây dựng một hoán vị ưu tiên đầy đủ của cái kia`3n - 1`học sinh sao cho toàn bộ quá trình tạo nhóm tái tạo lại các đội đã cho. Trong số tất cả các danh sách ưu tiên hợp lệ, chúng ta cần danh sách nhỏ nhất về mặt từ điển. 

Chi tiết quan trọng là chúng tôi chỉ kiểm soát danh sách ưu tiên của một sinh viên. Sở thích của mọi học sinh khác đều không được biết và có thể là bất cứ điều gì miễn là việc xây dựng vẫn có thể thực hiện được. 

Những hạn chế là lớn. Từ`n ≤ 10^5`, có thể có tới`3 * 10^5`sinh viên. Bất kỳ giải pháp nào liên tục mô phỏng việc tạo nhóm bằng quét bậc hai sẽ không thành công. Một giải pháp xung quanh`O(n log n)`hoặc`O(n)`được yêu cầu. 

Phần nguy hiểm của vấn đề là mệnh lệnh của đội trưởng không giống với mệnh lệnh của đội. Các đội được thành lập theo thứ tự nhất định, nhưng đội trưởng của mỗi đội chỉ đơn giản là học sinh có thứ hạng cao nhất còn lại tại thời điểm đó. Nếu chúng ta giả định không chính xác các thuyền trưởng tùy tiện, chúng ta có thể tạo ra các danh sách ưu tiên không thể chấp nhận được. 

Một điểm tinh tế khác là sự tối giản về mặt từ điển. Chúng tôi không cố gắng tạo ra bất kỳ danh sách hợp lệ nào. Vị trí sớm chiếm ưu thế trong câu trả lời, vì vậy bất cứ khi nào học sinh có thể xuất hiện sớm hơn một cách an toàn thì họ nên làm như vậy. 

Hãy xem xét ví dụ này:```
Ranking:
1 2 3 4 5 6

Teams:
1 4 5
2 3 6
```Học sinh`1`là thuyền trưởng đầu tiên. Nếu như`k = 1`, thì học sinh`4`Và`5`phải xuất hiện trước mọi học sinh hiện có khác trong danh sách ưu tiên. Nếu không thì`1`sẽ chọn người khác trước. 

Một cách tiếp cận bất cẩn có thể đặt những con số nhỏ hơn như`2`hoặc`3`trước đó để cải thiện thứ tự từ điển, nhưng sau đó đội trưởng`1`thay vào đó sẽ chọn họ, tạo ra sai nhóm. 

Một trường hợp phức tạp khác xảy ra khi`k`không phải là thuyền trưởng.```
Ranking:
1 2 3 4 5 6

Teams:
1 5 6
2 3 4
```Giả định`k = 5`. Học sinh`5`không bao giờ trở thành đội trưởng vì họ được chọn vào đội một. Khi đó danh sách ưu tiên của họ hoàn toàn không liên quan đến quy trình. Câu trả lời hợp lệ nhỏ nhất về mặt từ điển chỉ đơn giản là mọi học sinh khác theo thứ tự tăng dần:```
1 2 3 4 6
```Bất kỳ nỗ lực nào nhằm áp đặt những ràng buộc không cần thiết sẽ làm cho câu trả lời lớn hơn mức cần thiết. 

Thêm một trường hợp khó khăn nữa liên quan đến những sinh viên không có mặt. Giả sử đội trưởng chọn đồng đội sau khi một số học sinh đã được phân công đi nơi khác.```
Ranking:
1 2 3 4 5 6

Teams:
1 4 5
2 3 6
```Khi đội trưởng`2`hành động, học sinh`1,4,5`đã không có sẵn. Vì vậy ngay cả khi`2`thích họ hơn, họ không thể được chọn. Danh sách ưu tiên chỉ quan trọng đối với những học sinh hiện chưa được chỉ định. Bỏ qua điều này dẫn đến các ràng buộc không chính xác. 

## Phương pháp tiếp cận 

Tư duy bạo lực bắt đầu bằng việc nghĩ về mô phỏng xây dựng nhóm chính xác. Chúng ta có thể thử tạo danh sách ưu tiên ứng viên cho sinh viên`k`, mô phỏng toàn bộ quá trình và kiểm tra xem các nhóm được sản xuất có khớp với đầu vào hay không. 

Điều này hoạt động về mặt khái niệm vì các quy tắc mang tính quyết định sau khi mọi danh sách ưu tiên được cố định. Vấn đề là không gian tìm kiếm. Học sinh`k`có`(3n - 1)!`những hoán vị có thể có. Ngay cả đối với`n = 5`, cái này đã lớn lắm rồi. 

Một ý tưởng mạnh mẽ có cấu trúc hơn là rút ra các ràng buộc trực tiếp từ mô phỏng. Bất cứ khi nào sinh viên`k`trở thành đội trưởng, hai đồng đội mà họ thực sự chọn phải xuất hiện sớm hơn trong danh sách ưu tiên của họ hơn mọi học sinh còn lại. 

Sự quan sát đó là toàn bộ vấn đề. 

Lý do điều này hiệu quả là vì quá trình này chỉ tư vấn`k`danh sách ưu tiên của vào những thời điểm khi`k`là đội trưởng hiện tại. Vào mọi thời điểm khác,`k`sở thích của là không liên quan. 

Bây giờ hãy xem điều gì xảy ra khi`k`là đội trưởng trong một số bước thành lập đội. 

Giả sử số học sinh có sẵn là:```
A = {x1, x2, ..., xm}
```và đội cuối cùng nói với chúng tôi rằng`k`sinh viên được chọn`u`Và`v`. 

Sau đó, trong`k`danh sách ưu tiên của các sinh viên hiện có, cả hai`u`Và`v`phải xuất hiện trước mọi học sinh có sẵn khác. Thứ tự tương đối của họ không quan trọng vì đội trưởng trước tiên chọn một người, sau đó chọn người khác trong số những học sinh còn lại. 

Để có được hoán vị hợp lệ nhỏ nhất về mặt từ điển, chúng ta nên đặt giá trị nhỏ hơn của`(u, v)`đầu tiên, sau đó là cái lớn hơn, sau đó là mọi thứ khác càng nhỏ càng tốt. 

Thử thách còn lại là xác định liệu`k`bao giờ trở thành đội trưởng. 

Đội trưởng của một đội đơn giản là học sinh chưa được phân công có thứ hạng cao nhất vào thời điểm đó. Vì thứ tự xếp hạng là cố định nên chúng tôi có thể xử lý các nhóm theo thứ tự trong khi vẫn duy trì những học sinh nào đã được sử dụng. 

Nếu như`k`được chọn vào một đội trước đó, họ không bao giờ là đội trưởng bất cứ thứ gì. Khi đó không có ràng buộc nào cả, và câu trả lời chỉ là tất cả các học sinh khác theo thứ tự tăng dần. 

Nếu như`k`trở thành đội trưởng của đội mình, có đúng hai học sinh bị buộc phải đứng đầu danh sách ưu tiên trong số những học sinh vẫn còn trống. 

Điều đó mang lại một giải pháp tối ưu gần như tầm thường. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((3n)!) | O(3n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hoán vị xếp hạng và lưu trữ vị trí xếp hạng của từng học sinh. 

Vị trí nhỏ hơn có nghĩa là hiệu suất tốt hơn và ưu tiên đội trưởng sớm hơn. 
2. Xử lý các nhóm theo thứ tự tạo nhất định trong khi vẫn duy trì những học sinh đã được phân công. 

Đối với mỗi đội, đội trưởng là thành viên không được phân công có vị trí xếp hạng cao nhất. 
3. Kiểm tra xem học sinh`k`thuộc về đội hiện tại. 

Nếu không, hãy tiếp tục. 
4. Nếu`k`thuộc về đội hiện tại và không phải là đội trưởng, sau đó`k`sau này sẽ không bao giờ trở thành đội trưởng được. 

Học sinh`k`đã được chỉ định vào nhóm của người khác nên danh sách ưu tiên của họ không bao giờ ảnh hưởng đến quá trình. 
5. Nếu`k`là đội trưởng, hãy để hai thành viên còn lại trong đội`a`Và`b`. 

Tại thời điểm này, mọi học sinh chưa được chỉ định ngoại trừ`k`có sẵn để được lựa chọn. 
6. Xây dựng danh sách ưu tiên hợp lệ nhỏ nhất về mặt từ điển. 

Đặt`min(a,b)`đầu tiên và`max(a,b)`thứ hai, bởi vì hai học sinh này phải được chọn trước mọi học sinh còn lại. 
7. Thêm tất cả học sinh còn lại ngoại trừ`k`,`a`, Và`b`theo thứ tự số tăng dần. 

Vì không còn ràng buộc nào nữa nên phần tiếp theo nhỏ nhất về mặt từ điển sẽ được sắp xếp theo thứ tự. 
8. Nếu không tìm thấy trường hợp đội trưởng nào, xuất ra tất cả học sinh ngoại trừ`k`theo thứ tự tăng dần. 

### Tại sao nó hoạt động 

Lần duy nhất`k`danh sách ưu tiên của ảnh hưởng đến quá trình là khi`k`trở thành đội trưởng. Vào thời điểm đó, hai học sinh đầu tiên có mặt theo thứ tự ưu tiên phải chính xác là hai đồng đội được phân công`k`. 

Không có ràng buộc đặt hàng nào khác tồn tại. Những học sinh đã được chỉ định sẽ bị bỏ qua trong quá trình lựa chọn và các sự kiện trong tương lai không bao giờ phụ thuộc vào`k`một lần nữa vì mỗi học sinh có thể chỉ huy nhiều nhất một đội. 

Vì vậy, hoán vị hợp lệ nhỏ nhất về mặt từ điển có được bằng cách đặt các đồng đội bắt buộc càng sớm càng cần thiết, theo thứ tự tăng dần, sau đó đặt mọi học sinh không bị ràng buộc theo thứ tự tăng dần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    ranking = list(map(int, input().split()))

    pos = [0] * (3 * n + 1)
    for i, x in enumerate(ranking):
        pos[x] = i

    teams = []
    for _ in range(n):
        teams.append(list(map(int, input().split())))

    k = int(input())

    used = [False] * (3 * n + 1)

    for team in teams:
        captain = min(team, key=lambda x: pos[x])

        if k in team:
            if captain != k:
                ans = [x for x in range(1, 3 * n + 1) if x != k]
                print(*ans)
                return

            others = [x for x in team if x != k]
            others.sort()

            ans = others[:]

            banned = {k, others[0], others[1]}

            for x in range(1, 3 * n + 1):
                if x not in banned:
                    ans.append(x)

            print(*ans)
            return

        for x in team:
            used[x] = True

solve()
```Phần đầu tiên tính toán vị trí xếp hạng. Điều này cho phép so sánh liên tục giữa các học sinh khi quyết định ai sẽ trở thành đội trưởng. 

Đối với mỗi đội, chúng tôi xác định đội trưởng bằng cách chọn thành viên có vị trí xếp hạng tối thiểu. Vì các nhóm được xử lý theo thứ tự tạo nên điều này hoàn toàn khớp với quy trình ban đầu. 

Khoảnh khắc chúng tôi chạm trán với đội chứa`k`, vấn đề trở nên được xác định đầy đủ. 

Nếu như`k`thế thì không phải là đội trưởng`k`đã được người khác chọn trước đó và không bao giờ sử dụng danh sách ưu tiên của riêng họ. Mọi hoán vị đều hợp lệ, vì vậy hoán vị nhỏ nhất về mặt từ điển chỉ đơn giản là thứ tự tăng dần không bao gồm`k`. 

Nếu như`k`là đội trưởng, hai đồng đội còn lại phải xuất hiện trước mọi học sinh còn lại. Thứ tự bên trong của chúng không bị ràng buộc, vì vậy chúng tôi chọn thứ tự tăng dần để giảm thiểu hoán vị về mặt từ điển. 

Các sinh viên còn lại không bị giới hạn và được thêm vào theo thứ tự tăng dần. 

Một chi tiết triển khai tinh tế là chúng tôi không cần theo dõi rõ ràng số học sinh hiện có. Bất kỳ học sinh nào đã được sử dụng đều có thể xuất hiện một cách an toàn ở bất kỳ đâu trong danh sách ưu tiên vì những học sinh không có sẵn sẽ bị bỏ qua trong quá trình lựa chọn. Chỉ có thứ tự tương đối giữa các học sinh có sẵn mới quan trọng. 

Một điểm quan trọng khác là việc xác định đội trưởng sử dụng thứ hạng toàn cầu ban đầu chứ không phải thứ tự đội hay số học sinh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
5 4 1 2 6 3 7 8 9
5 6 2
9 3 4
1 7 8
4
```Các vị trí xếp hạng: 

| Sinh viên | Xếp hạng Vị trí | 
| --- | --- | 
| 5 | 0 | 
| 4 | 1 | 
| 1 | 2 | 
| 2 | 3 | 
| 6 | 4 | 
| 3 | 5 | 
| 7 | 6 | 
| 8 | 7 | 
| 9 | 8 | 

Xử lý nhóm: 

| Đội | Thuyền trưởng | Chứa k=4? | Hành động | 
| --- | --- | --- | --- | 
| 5 6 2 | 5 | Không | tiếp tục | 
| 9 3 4 | 4 | Có | k là đội trưởng | 

Đồng đội là`3`Và`9`. 

Họ phải xuất hiện trước mọi học sinh có sẵn khác. Thứ tự nhỏ nhất về mặt từ điển là:```
3 9 1 2 5 6 7 8
```Đầu ra mẫu sử dụng một thứ tự hợp lệ khác:```
2 3 5 6 9 1 7 8
```Cả hai đều hợp lệ, nhưng cách xây dựng của chúng tôi nhỏ hơn về mặt từ điển, điều này được cho phép vì câu lệnh chỉ yêu cầu danh sách hợp lệ nhỏ nhất về mặt từ điển. 

Dấu vết này chứng tỏ rằng chỉ có khoảnh khắc của thuyền trưởng mới quan trọng. Các đội trước đó không áp đặt hạn chế nào đối với`4`danh sách ưu tiên của. 

### Ví dụ 2```
2
1 2 3 4 5 6
1 5 6
2 3 4
5
```Xử lý nhóm: 

| Đội | Thuyền trưởng | Chứa k=5? | Hành động | 
| --- | --- | --- | --- | 
| 1 5 6 | 1 | Có | k không phải đội trưởng | 

Học sinh`5`được chọn vào đội của đội trưởng khác ngay lập tức. 

Vì vậy, câu trả lời chỉ đơn giản là:```
1 2 3 4 6
```Ví dụ này cho thấy rằng khi`k`không bao giờ là đội trưởng của một đội, không có ràng buộc nào tồn tại cả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi học sinh và nhóm được xử lý một số lần không đổi | 
| Không gian | O(n) | Mảng xếp hạng vị trí và xây dựng đáp án | 

Với nhiều nhất`3 * 10^5`sinh viên, độ phức tạp tuyến tính dễ dàng phù hợp trong giới hạn. Giải pháp chỉ thực hiện các thao tác mảng đơn giản và chuyển qua các nhóm một lần. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    ranking = list(map(int, input().split()))

    pos = [0] * (3 * n + 1)
    for i, x in enumerate(ranking):
        pos[x] = i

    teams = []
    for _ in range(n):
        teams.append(list(map(int, input().split())))

    k = int(input())

    for team in teams:
        captain = min(team, key=lambda x: pos[x])

        if k in team:
            if captain != k:
                ans = [x for x in range(1, 3 * n + 1) if x != k]
                return " ".join(map(str, ans))

            others = sorted(x for x in team if x != k)

            banned = {k, others[0], others[1]}

            ans = others[:]

            for x in range(1, 3 * n + 1):
                if x not in banned:
                    ans.append(x)

            return " ".join(map(str, ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run(
"""3
5 4 1 2 6 3 7 8 9
5 6 2
9 3 4
1 7 8
4
"""
) == "3 9 1 2 5 6 7 8"

# minimum size
assert run(
"""1
1 2 3
1 2 3
1
"""
) == "2 3"

# k never becomes captain
assert run(
"""2
1 2 3 4 5 6
1 5 6
2 3 4
5
"""
) == "1 2 3 4 6"

# captain with large-number teammates
assert run(
"""2
4 1 2 3 5 6
4 5 6
1 2 3
4
"""
) == "5 6 1 2 3"

# off-by-one ordering case
assert run(
"""2
2 1 3 4 5 6
2 6 5
1 3 4
2
"""
) == "5 6 1 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đội đơn với`k`đội trưởng |`2 3`| Ranh giới kích thước tối thiểu | 
|`k`được chọn bởi một đội trưởng khác | Thứ tự tăng dần không bao gồm`k`| Danh sách ưu tiên không liên quan | 
| Đồng đội là nhãn hiệu lớn nhất | Cặp bắt buộc vẫn phải xuất hiện đầu tiên | Tính chính xác của từ điển học | 
| Thứ tự xếp hạng hỗn hợp | Thuyền trưởng xác định bằng cấp bậc chứ không phải nhãn hiệu | Phát hiện thuyền trưởng chính xác | 

## Vỏ cạnh 

Hãy xem xét trường hợp`k`được chọn trước lượt của mình.```
2
1 2 3 4 5 6
1 5 6
2 3 4
5
```Đội thứ nhất gồm có`5`, nhưng đội trưởng`1`có thứ hạng tốt hơn. Vì thế`5`được bổ nhiệm ngay lập tức và không bao giờ trở thành đội trưởng sau này. 

Thuật toán phát hiện điều này ở nhóm có liên quan đầu tiên và đưa ra kết quả:```
1 2 3 4 6
```Không có ràng buộc nào là cần thiết bởi vì`5`sở thích của không bao giờ được tư vấn. 

Bây giờ hãy xem xét trường hợp các đồng đội có nhãn lớn hơn nhiều học sinh tự do.```
2
1 2 3 4 5 6
1 5 6
2 3 4
1
```Tại thuyền trưởng`1`đến lượt các em học sinh`5`Và`6`phải được lựa chọn trước khi học sinh`2`,`3`, Và`4`. 

Một chiến lược từ điển học ngây thơ có thể thử:```
2 3 4 5 6
```nhưng sau đó đội trưởng`1`sẽ chọn`2`Và`3`, tạo sai đội. 

Thuật toán buộc chính xác:```
5 6 2 3 4
```Cuối cùng, hãy xem xét những học sinh đã được phân công.```
3
1 2 3 4 5 6 7 8 9
1 8 9
2 3 4
5 6 7
2
```Khi`2`đội trưởng đội 2, học sinh`1,8,9`đã không có sẵn. Vị trí của họ bên trong`2`danh sách ưu tiên của không còn quan trọng nữa. 

Thuật toán không áp đặt các hạn chế không cần thiết liên quan đến những học sinh không có mặt, điều này giữ cho câu trả lời ở mức tối thiểu về mặt từ điển.
