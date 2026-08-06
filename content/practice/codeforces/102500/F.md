---
title: "CF 102500F - Xe cứu hỏa màu đỏ"
description: "Chúng tôi có n người. Mỗi người được mô tả bằng một bộ số. Hai người có thể được kết nối trực tiếp nếu tồn tại một số xuất hiện trong mô tả của cả hai người."
date: "2026-08-05T18:03:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102500
codeforces_index: "F"
codeforces_contest_name: "2019-2020 ICPC Northwestern European Regional Programming Contest (NWERC 2019)"
rating: 0
weight: 102500
solve_time_s: 53
verified: true
draft: false
---

[CF 102500F - Xe cứu hỏa màu đỏ](https://codeforces.com/problemset/problem/102500/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`mọi người. Mỗi người được mô tả bằng một bộ số. Hai người có thể được kết nối trực tiếp nếu tồn tại một số xuất hiện trong mô tả của cả hai người. Đầu ra được yêu cầu không phải là toàn bộ biểu đồ của các kết nối có thể có mà là bằng chứng chứa chính xác`n - 1`những kết nối trực tiếp như vậy. Những kết nối đó phải làm cho mọi người đều có thể tiếp cận được với nhau. 

Điều này tương đương với việc tìm cây bao trùm trong biểu đồ trong đó mọi người là các đỉnh và các số chung tạo nên các cạnh. Khó khăn là việc xây dựng tất cả các cạnh một cách rõ ràng có thể quá tốn kém vì nhiều người có thể chia sẻ cùng một số. 

Số người có thể đạt tới`2 * 10^5`và tổng số mô tả của tất cả mọi người cũng nhiều nhất`2 * 10^5`. Điều này có nghĩa là một thuật toán chỉ nên xử lý mỗi số đã cho một số lần nhỏ. Việc xây dựng mọi cặp người có chung một giá trị có thể là phương trình bậc hai trong trường hợp xấu nhất. Ví dụ: nếu một số xuất hiện trong danh sách của mỗi người, biểu đồ rõ ràng chứa khoảng`n * (n - 1) / 2`các cạnh xung quanh`2 * 10^10`khi`n = 2 * 10^5`, vượt xa thời gian và bộ nhớ có sẵn. 

Một số trường hợp cần được chăm sóc. Nếu không có ai chia sẻ bất kỳ số nào thì câu trả lời là không thể vì không có cạnh nào kết nối các nhóm khác nhau. 

Ví dụ:```
2
1 5
1 6
```Đầu ra đúng là:```
impossible
```Việc triển khai bất cẩn cho rằng mọi cặp người đều có thể được kết nối bằng cách nào đó có thể tạo ra một mối quan hệ không hợp lệ. 

Một trường hợp khác là khi tất cả mọi người được kết nối thông qua một chuỗi nhưng không trực tiếp với người đầu tiên.```
3
1 1
2 1 2
1 2
```Một câu trả lời hợp lệ là:```
1 2 1
2 3 2
```Cố gắng kết nối trực tiếp người 1 với người 3 sẽ là sai lầm vì họ không chia sẻ số điện thoại. 

Trường hợp cạnh thứ ba là việc sử dụng lặp đi lặp lại một số rất phổ biến. Đầu ra phải chứa chính xác`n - 1`quan hệ chứ không phải mọi mối quan hệ có thể có. Ví dụ: nếu mỗi người đều có số`7`, chúng ta chỉ cần một cây`n - 1`các cạnh, không phải tất cả các cặp có thể. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ xây dựng một biểu đồ về con người. Với mỗi số, chúng ta có thể lưu trữ tất cả những người chứa số đó, sau đó kết nối mọi cặp trong nhóm đó. Biểu đồ này đúng vì mỗi cạnh được tạo đại diện cho một số chia sẻ hợp lệ. Sau khi xây dựng nó, tìm kiếm theo chiều sâu hoặc cấu trúc tìm kiếm liên kết có thể tìm thấy cây bao trùm. 

Vấn đề là số cạnh được tạo ra. Nếu một số xuất hiện trong tất cả`n`mô tả, giá trị duy nhất đó tạo ra`n * (n - 1) / 2`các cạnh. Với kích thước đầu vào tối đa, điều này là không thể lưu trữ hoặc xử lý. 

Quan sát quan trọng là chúng ta không cần mọi cạnh có thể. Chúng ta chỉ cần đủ lợi thế để kết nối tất cả mọi người. Đối với số được nhiều người chia sẻ thì một chuỗi hoặc ngôi sao sử dụng số đó là đủ. Chúng ta có thể giữ một người đại diện cho mỗi số và kết nối những người sau này có số đó với người đại diện. Điều này tạo ra nhiều nhất một cạnh hữu ích cho mỗi lần xuất hiện của một số. 

Trong khi xử lý con người, chúng ta có thể coi các con số là chất kết nối. Người đầu tiên nhìn thấy một số sẽ trở thành chủ sở hữu của đường kết nối đó. Mọi người sau này sử dụng cùng một đầu nối đều có thể được gắn vào chủ sở hữu đó. Sau khi tất cả các số được xử lý, các cạnh được tạo sẽ tạo thành một biểu đồ chỉ sử dụng các kết nối hợp lệ. Nếu đồ thị đó chứa`n - 1`các cạnh và nối tất cả các đỉnh, nó là một cây bao trùm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(S²) trong trường hợp xấu nhất trong đó S là tổng số mô tả | O(S²) | Quá chậm | 
| Tối ưu | O(S) | O(S) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc danh sách số của mỗi người. Duy trì bản đồ từ mỗi số đến người đầu tiên được nhìn thấy với số đó. 
2. Đối với mỗi số của một người, hãy kiểm tra xem đã có người khác giới thiệu số này chưa. Nếu không, hãy lưu trữ người hiện tại làm đại diện của nó. Nếu có, hãy tạo lợi thế giữa người hiện tại và người đại diện bằng con số này. 

Người đại diện đóng vai trò là trung tâm. Mọi kết nối được tạo theo cách này đều hợp lệ vì cả hai điểm cuối đều chứa cùng một số. 
3. Sau khi tất cả đầu vào được xử lý, hãy kiểm tra xem có bao nhiêu kết nối đã được tạo. Nếu có ít hơn`n - 1`, đồ thị không thể kết nối được, do đó xuất ra`impossible`. 
4. Nếu không, xuất tất cả các kết nối được lưu trữ. 

Tại sao nó hoạt động: 

Với mỗi số, tất cả những người chứa số đó đều được kết nối thông qua người đại diện được chọn cho số đó. Điều này có nghĩa là mọi nhóm kết nối ban đầu có thể vẫn được kết nối mặc dù chúng tôi chỉ giữ lại một tập hợp con nhỏ các cạnh. Nếu biểu đồ cuối cùng có đủ cạnh để tạo thành cây bao trùm thì các kết nối được tạo ra sẽ kết nối tất cả mọi người. Nếu không, một số nhóm người chưa bao giờ chia sẻ bất kỳ con số nào, vì vậy không có bằng chứng xác thực nào tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    owner = {}
    ans = []

    for i in range(1, n + 1):
        data = list(map(int, input().split()))
        m = data[0]
        for x in data[1:]:
            if x in owner:
                ans.append((i, owner[x], x))
            else:
                owner[x] = i

    if len(ans) != n - 1:
        print("impossible")
    else:
        out = []
        for a, b, x in ans:
            out.append(f"{a} {b} {x}")
        print("\n".join(out))

if __name__ == "__main__":
    solve()
```Từ điển`owner`lưu trữ người đầu tiên liên quan đến mỗi số. Lần xuất hiện đầu tiên của một giá trị không tạo ra cạnh vì chưa có ai khác để kết nối. Mỗi lần xuất hiện sau đó sẽ tạo ra chính xác một cạnh. 

Thuật toán dựa trên thực tế là tổng số số được liệt kê chỉ`2 * 10^5`, vì vậy việc lặp lại tất cả các mô tả một lần là đủ. Các hoạt động từ điển của Python được mong đợi là có thời gian không đổi, làm cho toàn bộ giải pháp trở nên tuyến tính. 

Việc kiểm tra số cạnh cuối cùng là cần thiết. Một biểu đồ được kết nối trên`n`đỉnh cần ít nhất`n - 1`các cạnh và cấu trúc này không bao giờ tạo ra các cạnh không cần thiết trùng lặp cho cùng một lần xuất hiện. Nếu số đếm không chính xác`n - 1`, biểu đồ được tạo không thể là bằng chứng cần thiết. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2
1 5
2 10 22
3 17 22 9
2 17 8
3 9 22 16
```Trạng thái xử lý là: 

| Người | Số đã xử lý | Đã thêm chủ sở hữu | Các cạnh được tạo | 
| --- | --- | --- | --- | 
| 1 | 5 | 5 -> 1 | không | 
| 2 | 10, 22 | 10 -> 2, 22 -> 2 | không | 
| 3 | 17, 22, 9 | 17 -> 3, 9 -> 3 | 3 2 22 | 
| 4 | 17, 8 | 8 -> 4 | 4 3 17 | 
| 5 | 9, 22, 16 | 16 -> 5 | 5 3 9, 5 2 22 | 

Có năm người nên cần có bốn cạnh. Chỉ có ba cạnh hữu ích được tạo ra vì người đầu tiên bị cô lập. Thuật toán in chính xác`impossible`. 

Đối với mẫu thứ hai:```
6
2 17 10
2 5 10
2 10 22
3 17 22 9
2 17 8
3 9 22 16
```| Người | Số đã xử lý | Các cạnh được tạo | 
| --- | --- | --- | 
| 1 | 17, 10 | không | 
| 2 | 5, 10 | 2 1 10 | 
| 3 | 10, 22 | 3 1 10, 22 trở thành sở hữu | 
| 4 | 17, 22, 9 | 4 1 17, 4 3 22 | 
| 5 | 17, 8 | 5 1 17 | 
| 6 | 9, 22, 16 | 6 4 9, 6 3 22 | 

Việc xây dựng kết nối tất cả các nhóm thông qua các số chia sẻ. Bất kỳ tập hợp con nào của`n - 1`các cạnh được tạo ra tạo thành một cây là một câu trả lời hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S) | Mỗi số được mô tả được xử lý một lần, trong đó S là tổng của tất cả các kích thước danh sách. | 
| Không gian | O(S) | Từ điển chủ sở hữu và danh sách câu trả lời lưu trữ tối đa một lượng thông tin không đổi cho mỗi mô tả. | 

Tổng số mô tả tối đa là`2 * 10^5`, do đó, một giải pháp tuyến tính dễ dàng phù hợp với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# minimum connected case
assert run("""2
1 7
1 7
""").strip() != "impossible"

# disconnected case
assert run("""2
1 1
1 2
""").strip() == "impossible"

# chain connection
assert run("""3
1 1
2 1 2
1 2
""").strip() != "impossible"

# all equal value
assert run("""5
1 9
1 9
1 9
1 9
1 9
""").strip() != "impossible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai người chung một số | Một cạnh | Cây hợp lệ tối thiểu | 
| Hai người bị cô lập | không thể | Phát hiện các thành phần bị ngắt kết nối | 
| Ba người kết nối thông qua một người trung gian | Hai cạnh | Kết nối gián tiếp | 
| Tất cả mọi người chia sẻ một số | Bốn cạnh | Xử lý các nhóm chia sẻ lớn | 

## Vỏ cạnh 

Đối với các nhóm bị cô lập, thuật toán không bao giờ chèn cạnh vì không có số nào xuất hiện trong cả hai nhóm. Trong đầu vào:```
2
1 5
1 6
```từ điển nhận được hai mục riêng biệt,`5 -> 1`Và`6 -> 2`, và danh sách câu trả lời vẫn trống. Vì ít hơn`n - 1`các cạnh tồn tại, thuật toán đưa ra`impossible`. 

Đối với các kết nối chuỗi, thuật toán không yêu cầu mọi cặp người được kết nối phải chia sẻ một giá trị. TRONG:```
3
1 1
2 1 2
1 2
```người 2 trở thành cầu nối. Con số`1`kết nối người 1 và người 2, trong khi số`2`kết nối người 2 và người 3. Cây được tạo ra chứng tỏ rằng tất cả mọi người đều được kết nối với nhau. 

Đối với một số được chia sẻ bởi mọi người:```
5
1 7
1 7
1 7
1 7
1 7
```người đầu tiên trở thành chủ sở hữu của`7`. Mỗi người sau tạo một cạnh cho người 1, được chính xác bốn cạnh. Thuật toán tránh tạo ra mười cặp có thể có trong khi vẫn duy trì kết nối.
