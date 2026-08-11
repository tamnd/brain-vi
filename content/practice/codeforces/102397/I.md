---
title: "CF 102397I - Dr.Hjjawi và MCQ"
description: "Có n câu hỏi trắc nghiệm và Ayoub trả lời mỗi câu hỏi bằng một chữ cái từ a đến e. Thông tin bổ sung quan trọng là câu trả lời đúng phải có cùng một chữ cái cho mọi câu hỏi. Chúng ta không biết đó là chữ nào trong năm chữ cái đó."
date: "2026-08-10T18:13:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102397
codeforces_index: "I"
codeforces_contest_name: "Asu Coding Cup 4"
rating: 0
weight: 102397
solve_time_s: 322
verified: true
draft: false
---

[CF 102397I - Dr.Hjjawi và MCQ](https://codeforces.com/problemset/problem/102397/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

có`n`câu hỏi trắc nghiệm và Ayoub trả lời mỗi câu hỏi bằng một lá thư từ`a`bởi vì`e`. Thông tin bổ sung quan trọng là câu trả lời đúng phải có cùng một chữ cái cho mọi câu hỏi. Chúng ta không biết đó là chữ nào trong năm chữ cái đó. 

Đối với bất kỳ lựa chọn cố định nào về chữ cái đúng, Ayoub nhận được câu hỏi đúng một cách chính xác khi câu trả lời của anh ấy ở vị trí đó bằng với chữ cái đã chọn. Vì vậy, nếu chữ cái đúng là`c`, điểm số của anh ấy chỉ đơn giản là số lượng`c`các ký tự trong chuỗi câu trả lời của anh ấy. 

Chúng ta cần xem xét tất cả năm chữ cái có thể đúng. Điểm tối thiểu có thể có là tần suất nhỏ nhất trong số`a`,`b`,`c`,`d`, Và`e`, trong khi điểm tối đa có thể có là tần số lớn nhất. 

Số lượng câu hỏi thỏa mãn`1 <= n <= 1000`. Đây là kích thước đầu vào rất nhỏ, do đó, ngay cả việc kiểm tra tất cả năm chữ cái có thể đúng đối với mỗi câu hỏi cũng chỉ cần`5 * 1000 = 5000`so sánh nhân vật. Quét tuyến tính là quá đủ nhanh và ngay cả giải pháp năm bước đơn giản cũng có thể thoải mái trong giới hạn thời gian. Không cần sắp xếp, lập trình động hoặc bất kỳ cấu trúc dữ liệu phức tạp nào hơn. 

Có một số trường hợp việc triển khai có thể diễn ra sai sót một cách âm thầm. Với đầu vào`3`Và`aaa`, câu trả lời là`0 3`. Nếu chữ đúng là`a`, cả ba đáp án đều đúng, nhưng nếu là bất kỳ chữ cái nào khác thì không có đáp án nào đúng. Việc triển khai bất cẩn giả định đúng chữ cái phải xuất hiện trong chuỗi có thể tạo ra sai số tối thiểu`1`. 

Một trường hợp đặc biệt khác là khi mọi chữ cái có thể xuất hiện thường xuyên như nhau. Ví dụ, với`5`câu hỏi và chuỗi`abcde`, mỗi chữ cái đúng có thể cho chính xác một câu trả lời đúng, do đó kết quả là`1 1`. Việc triển khai khởi tạo mức tối thiểu hoặc tối đa từ tần số sai có thể thất bại trong trường hợp này. 

Đầu vào hợp lệ nhỏ nhất cũng hữu ích. Vì`n = 1`Và`s = a`, số điểm có thể có là`1, 0, 0, 0, 0`, vậy câu trả lời là`0 1`. Bốn chữ cái không xuất hiện trong chuỗi vẫn phải được xem xét. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp có thể thử từng chữ cái trong số năm chữ cái đúng có thể có. Đối với một chữ cái được chọn, nó sẽ quét toàn bộ chuỗi câu trả lời và đếm xem có bao nhiêu vị trí chứa chữ cái đó. Sau khi làm điều này cho`a`,`b`,`c`,`d`, Và`e`, phải có điểm tối thiểu và tối đa trong năm điểm. Điều này đúng vì tuyên bố đảm bảo rằng bài kiểm tra thực tế có chính xác một chữ cái đúng chung và chỉ có năm lựa chọn khả thi cho nó. 

Trường hợp xấu nhất thực hiện`5n`so sánh nhân vật. Với`n <= 1000`, đó là nhiều nhất`5000`so sánh, do đó phương pháp vũ lực này không thực sự trở nên quá chậm trong các ràng buộc nhất định. Độ phức tạp tiệm cận của nó vẫn còn`O(n)`, vì hệ số của năm là hằng số. 

Chúng ta có thể làm cho lý luận tương tự trở nên rõ ràng hơn bằng cách quan sát rằng điểm của một chữ cái ứng cử viên chính xác là tần số của nó trong chuỗi. Thay vì quét toàn bộ chuỗi một lần cho mỗi ứng viên, chúng ta có thể quét chuỗi một lần và duy trì năm bộ đếm. Mỗi ký tự tăng chính xác một bộ đếm. Khi đã biết tần số, tần số tối thiểu và tối đa sẽ ngay lập tức đưa ra câu trả lời cần thiết. 

Brute-force hoạt động vì chỉ có năm câu trả lời đúng, nhưng nó liên tục kiểm tra các ký tự giống nhau. Việc quan sát thấy điểm của một ứng viên chỉ đơn giản là tần số của nó cho phép chúng tôi thu thập tất cả năm điểm trong một lần quét. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(5n) = O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

Phiên bản tối ưu được ưu tiên hơn vì mối quan hệ của nó với bài toán là trực tiếp: đếm từng chữ cái một lần, sau đó kiểm tra năm lần đếm. 

## Hướng dẫn thuật toán 

1. Tạo năm bộ đếm đại diện cho tần số của`a`,`b`,`c`,`d`, Và`e`. Mỗi câu trả lời đúng có thể có đều cần có số đếm riêng vì mỗi số đếm đại diện cho số điểm mà Ayoub sẽ nhận được nếu chữ cái đó là câu trả lời đúng chung. 
2. Quét từng ký tự trong chuỗi câu trả lời và tăng bộ đếm tương ứng với ký tự đó. Sau khi xử lý bất kỳ tiền tố nào của chuỗi, mỗi bộ đếm bằng số lần chữ cái của nó xuất hiện trong tiền tố đó. 
3. Sau khi toàn bộ chuỗi đã được xử lý, hãy tìm số nhỏ nhất và lớn nhất trong năm bộ đếm. Các giá trị này chính xác là điểm tối thiểu và tối đa vì việc chọn một chữ cái cụ thể làm câu trả lời đúng sẽ mang lại cho Ayoub câu trả lời đúng chính xác tại các vị trí chứa chữ cái đó. 
4. In tần số tối thiểu theo sau là tần số tối đa. 

### Tại sao nó hoạt động 

hãy để`count[x]`là số câu hỏi Ayoub trả lời bằng lá thư`x`. Nếu như`x`là câu trả lời đúng thực sự phổ biến, Ayoub đúng chính xác ở những câu hỏi mà câu trả lời của anh ấy là`x`, vậy số điểm của anh ấy là`count[x]`. Vì câu trả lời đúng chung có thể là bất kỳ chữ cái nào trong số năm chữ cái, nên tập hợp tất cả các điểm có thể có chính xác là năm tần số. Lấy mức tối thiểu sẽ cho điểm nhỏ nhất có thể và lấy mức tối đa sẽ cho điểm lớn nhất có thể. Quá trình đếm tính toán chính xác từng tần số trong số này, vì vậy cặp cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
s = input().strip()

count = [0] * 5

for ch in s:
    count[ord(ch) - ord('a')] += 1

print(min(count), max(count))
```Dòng đầu tiên ghi`n`, mặc dù thuật toán không cần nó sau khi đọc chuỗi. Bản thân chuỗi chứa tất cả thông tin cần thiết để tính năm điểm có thể. 

các`count`mảng có năm vị trí. Vì đầu vào chỉ chứa các chữ cái từ`a`bởi vì`e`, biểu thức`ord(ch) - ord('a')`ánh xạ các ký tự này vào các chỉ mục`0`bởi vì`4`. Như vậy`a`cập nhật`count[0]`,`b`cập nhật`count[1]`, vân vân. 

Vòng lặp xử lý mỗi ký tự chính xác một lần. Không cần phải so sánh từng ký tự với cả năm chữ cái vì ký tự riêng của nó xác định chính xác bộ đếm nào phải tăng. 

Việc khởi tạo số 0 rất quan trọng vì một chữ cái có thể không bao giờ xuất hiện. Ví dụ, với`aaa`, bộ đếm trở thành`[3, 0, 0, 0, 0]`. Các giá trị 0 đó thể hiện các khả năng hợp lệ trong đó không có chữ cái đúng trong câu trả lời của Ayoub. 

Số nguyên Python không có vấn đề tràn ở đây và tối đa mọi bộ đếm đều có`1000`. Chuỗi đầu vào bị loại bỏ để dòng mới từ đầu vào tiêu chuẩn không được coi là ký tự câu trả lời. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
10
aaaaaabcde
```Chuỗi chứa sáu`a`các ký tự và một lần xuất hiện của mỗi ký tự`b`,`c`,`d`, Và`e`. Quá trình quét tiến triển như sau. 

| Ký tự đã xử lý | một | b | c | d | e | 
| --- | --- | --- | --- | --- | --- | 
|`a`| 1 | 0 | 0 | 0 | 0 | 
|`a`| 2 | 0 | 0 | 0 | 0 | 
|`a`| 3 | 0 | 0 | 0 | 0 | 
|`a`| 4 | 0 | 0 | 0 | 0 | 
|`a`| 5 | 0 | 0 | 0 | 0 | 
|`a`| 6 | 0 | 0 | 0 | 0 | 
|`b`| 6 | 1 | 0 | 0 | 0 | 
|`c`| 6 | 1 | 1 | 0 | 0 | 
|`d`| 6 | 1 | 1 | 1 | 0 | 
|`e`| 6 | 1 | 1 | 1 | 1 | 

Năm điểm có thể có là`6, 1, 1, 1, 1`. Do đó tối thiểu là`1`và tối đa là`6`, sản xuất`1 6`. 

Dấu vết này thể hiện tính bất biến trung tâm: mỗi bộ đếm luôn lưu trữ số lần xuất hiện của câu trả lời tương ứng. 

### Mẫu 2 

Đầu vào là:```
3
aaa
```Quá trình quét đưa ra trạng thái sau. 

| Ký tự đã xử lý | một | b | c | d | e | 
| --- | --- | --- | --- | --- | --- | 
|`a`| 1 | 0 | 0 | 0 | 0 | 
|`a`| 2 | 0 | 0 | 0 | 0 | 
|`a`| 3 | 0 | 0 | 0 | 0 | 

Số điểm có thể là`3, 0, 0, 0, 0`. Tối thiểu là`0`và tối đa là`3`, vì vậy đầu ra là`0 3`. 

Ví dụ này thực hiện trường hợp có thể có bốn chữ cái đúng không bao giờ xuất hiện trong chuỗi câu trả lời. Tần số 0 của chúng không được loại bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự trong chuỗi câu trả lời được xử lý một lần. | 
| Không gian | O(1) | Chỉ có năm bộ đếm tần số được lưu trữ, bất kể`n`. | 

Với`n <= 1000`, thuật toán chỉ thực hiện một nghìn lần cập nhật ký tự và một lượng công việc bổ sung không đổi. Nó thấp hơn nhiều so với giới hạn thời gian khả dụng và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    s = input().strip()

    count = [0] * 5

    for ch in s:
        count[ord(ch) - ord('a')] += 1

    print(min(count), max(count))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()
    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

# Provided samples
assert run("10\naaaaaabcde\n") == "1 6", "sample 1"
assert run("3\naaa\n") == "0 3", "sample 2"

# Minimum-size input
assert run("1\na\n") == "0 1", "single question"

# All five letters occur equally often
assert run("5\nabcde\n") == "1 1", "all letters equally frequent"

# All answers are the same
assert run("7\nbbbbbbb\n") == "0 7", "all equal values"

# Maximum-size input
assert run("1000\n" + "a" * 1000 + "\n") == "0 1000", "maximum n"

# Boundary distribution that catches incorrect min/max initialization
assert run("5\naaaab\n") == "0 4", "zero-frequency letters and maximum frequency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a`|`0 1`| Kích thước đầu vào tối thiểu và các chữ cái vắng mặt | 
|`5 / abcde`|`1 1`| Tần số bằng nhau và tối thiểu và tối đa bằng nhau | 
|`7 / bbbbbbb`|`0 7`| Tất cả các câu trả lời đều giống nhau | 
|`1000 / a...a`|`0 1000`| Kích thước đầu vào tối đa và tần số lớn | 
|`5 / aaaab`|`0 4`| Ứng viên có tần số bằng 0 và cực trị đúng | 

## Vỏ cạnh 

Đối với trường hợp một câu hỏi, hãy xem xét`n = 1`Và`s = a`. Bộ đếm trở thành`[1, 0, 0, 0, 0]`. Nếu như`a`là câu trả lời đúng, Ayoub được một điểm. Nếu bất kỳ chữ cái nào khác đúng, anh ta sẽ không được điểm. Thuật toán in`0 1`, tính toán chính xác tất cả năm câu trả lời đúng có thể có. 

Đối với chuỗi câu trả lời chỉ chứa một chữ cái riêng biệt, hãy xem xét`n = 3`Và`s = aaa`. Bộ đếm trở thành`[3, 0, 0, 0, 0]`. Điểm tối đa là`3`, thu được khi`a`là đúng, trong khi mức tối thiểu là`0`, thu được từ bất kỳ chữ cái nào trong số bốn chữ cái có thể đúng còn lại. Kết quả là`0 3`. 

Đối với trường hợp mỗi chữ cái xuất hiện đúng một lần, hãy xem xét`n = 5`Và`s = abcde`. Các quầy là`[1, 1, 1, 1, 1]`. Bất kể chữ cái nào là câu trả lời đúng nhất, Ayoub đều trả lời đúng một câu hỏi. Do đó cả hai cực trị đều`1`, cho`1 1`. 

Để có kích thước đầu vào tối đa, hãy xem xét`n = 1000`và một chuỗi chứa một nghìn`a`nhân vật. Các quầy là`[1000, 0, 0, 0, 0]`, vậy kết quả là`0 1000`. Vòng lặp vẫn chỉ thực hiện một thao tác cho mỗi ký tự, chứng tỏ rằng giải pháp sẽ chia tỷ lệ trực tiếp với kích thước đầu vào.
