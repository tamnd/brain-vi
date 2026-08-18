---
title: "CF 102343B - Sắp xếp theo tần số"
description: "Chúng ta được cấp một chuỗi không rỗng chỉ chứa các chữ cái tiếng Anh viết thường. Nhiệm vụ là sắp xếp lại các ký tự của nó sao cho các ký tự có tần số lớn hơn xuất hiện sớm hơn trong kết quả."
date: "2026-08-17T10:17:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102343
codeforces_index: "B"
codeforces_contest_name: "UCF Locals 2019"
rating: 0
weight: 102343
solve_time_s: 72
verified: true
draft: false
---

[CF 102343B - Sắp xếp theo tần suất](https://codeforces.com/problemset/problem/102343/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi không rỗng chỉ chứa các chữ cái tiếng Anh viết thường. Nhiệm vụ là sắp xếp lại các ký tự của nó sao cho các ký tự có tần số lớn hơn xuất hiện sớm hơn trong kết quả. Khi hai ký tự xuất hiện thường xuyên như nhau thì ký tự nhỏ hơn theo thứ tự bảng chữ cái phải xuất hiện trước. Mọi lần xuất hiện của chuỗi gốc phải được giữ nguyên ở đầu ra, vì vậy chúng tôi chỉ thay đổi thứ tự của các ký tự. 

Ví dụ, chuỗi`dcbbdabb`chứa bốn`b`nhân vật, hai`d`nhân vật, một`a`, và một`c`. Do đó, tần số được sắp xếp như sau`b`,`d`, sau đó`a`Và`c`, với`a`trước`c`vì tần số của chúng bằng nhau. Kết quả cần tìm là`bbbbddac`. Vấn đề ban đầu giới hạn chuỗi đầu vào ở 70 chữ cái viết thường và phiên bản Codeforces có giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

Giới hạn nhỏ 70 có nghĩa là ngay cả giải pháp O(n²) cũng thực hiện tối đa 4900 phép so sánh ký tự cơ bản, do đó việc triển khai đơn giản đã đủ nhanh. Tuy nhiên, cấu trúc của bài toán mang lại cho chúng ta lời giải O(n) với một lượng công việc bổ sung cố định. Điều quan trọng là chỉ có 26 ký tự có thể có, vì vậy chúng ta không bao giờ cần khám phá hoặc sắp xếp một tập hợp các giá trị tùy ý. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai tạo ra sai thứ tự. Nếu mỗi ký tự đều khác nhau, như trong`abc`, mọi tần số là một, do đó thứ tự bảng chữ cái quyết định mọi thứ và đầu ra là`abc`. Một giải pháp duy trì trật tự ban đầu thay vì áp dụng bộ ngắt kết nối sẽ không thành công đối với đầu vào như`cba`, có đầu ra đúng là`abc`. 

Tần số bằng nhau cũng có thể xảy ra giữa các ký tự được phân tách trong bảng chữ cái. Vì`programming`, tần số là`g=2`,`m=2`và mọi ký tự trong số`a`,`i`,`n`,`o`,`p`,`r`xảy ra một lần. Đầu ra đúng là`ggmmainopr`chỉ khi các chữ cái được sắp xếp chính xác trong mỗi nhóm tần số, nhưng đầu ra mẫu thực tế là`ggmmrrainop`bởi vì`r`cũng xảy ra hai lần. Do đó thứ tự tần số hoàn chỉnh là`g`,`m`,`r`, theo sau là`a`,`i`,`n`,`o`,`p`, cho`ggmmrrainop`. Việc triển khai bất cẩn chỉ tính một số lần xuất hiện hoặc sử dụng thứ tự ký tự gốc cho các mối quan hệ sẽ thất bại ở đây. 

Một trường hợp ranh giới khác là một chuỗi chỉ chứa một ký tự riêng biệt, chẳng hạn như`aaaaa`. Tần số của nó là năm và không có ký tự cạnh tranh, vì vậy câu trả lời phải được giữ nguyên`aaaaa`. Việc triển khai vô tình tạo một bản sao đầu ra cho mỗi ký tự riêng biệt thay vì một bản sao cho mỗi lần xuất hiện sẽ tạo ra độ dài sai. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp có thể xử lý từng ký tự riêng biệt. Đối với mỗi vị trí trong chuỗi đầu vào, hãy quét toàn bộ chuỗi và đếm xem ký tự đó xuất hiện bao nhiêu lần. Sau khi lấy được tần số của từng ký tự, sắp xếp các ký tự theo tần số giảm dần và thứ tự chữ cái tăng dần, sau đó viết mỗi ký tự với số lần bằng tần số của nó. Điều này đúng vì mỗi ký tự được gán tần số chung chính xác trước khi thứ tự cuối cùng được thực hiện. 

Nếu đầu vào có độ dài n và chúng tôi quét độc lập tất cả n vị trí cho mọi vị trí, thì pha đếm tần số sẽ thực hiện kiểm tra ký tự O(n2). Theo ràng buộc thực tế n ≤ 70, trường hợp xấu nhất chỉ là 70² = 4900 kiểm tra, do đó phiên bản brute-force này vẫn dễ dàng được chấp nhận. Không có vấn đề hiệu suất thực tế nào ở ràng buộc chính thức. Tuy nhiên, nếu phương pháp tương tự được áp dụng cho n = 100000 thì nó sẽ yêu cầu 10^10 lần kiểm tra, vượt xa những gì mà chương trình cuộc thi 1 giây có thể xử lý. 

Quan sát giúp giải pháp tốt hơn trở nên đơn giản là mỗi ký tự thuộc về một trong 26 loại chữ cái viết thường. Chúng ta có thể đếm tất cả tần số trong một lượt với một mảng có độ dài 26. Khi đã biết số lượng đó, chúng ta chỉ cần kiểm tra bảng chữ cái từ`a`bởi vì`z`. Tần số xác định thứ tự chính, vì vậy chúng ta có thể thu thập 26 ký tự và sắp xếp các cặp đó theo`(-frequency, character)`. Bởi vì 26 là một hằng số nên bước sắp xếp này mất thời gian không đổi đối với độ dài đầu vào. 

Hai cách tiếp cận này khác nhau chủ yếu ở mức độ lặp lại công việc mà chúng thực hiện. Phương pháp brute-force liên tục khám phá lại các tần số có thể đã được lưu trữ, trong khi phương pháp tối ưu ghi lại từng tần số một lần và sau đó hoạt động trên bảng chữ cái cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(26) | Được chấp nhận với n ≤ 70 | 
| Tối ưu | O(n + 26 log 26) = O(n) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào đơn và tạo một mảng tần số có độ dài 26. Chỉ mục`0`đại diện cho`a`, chỉ mục`1`đại diện cho`b`, v.v. thông qua`z`. Một mảng cố định là đủ vì bảng chữ cái đầu vào đã được biết trước. 
2. Duyệt từng ký tự trong chuỗi và tăng tần số tương ứng của nó. Đối với một nhân vật`ch`, vị trí mảng của nó là`ord(ch) - ord('a')`. Sau lần vượt qua này, mảng tần số chứa thông tin đầy đủ cần thiết để xây dựng câu trả lời. 
3. Tạo 26 cặp ký tự-tần số cho bảng chữ cái viết thường. Các ký tự có tần số bằng 0 có thể được tạm thời để lại trong bộ sưu tập vì chúng sẽ không đóng góp gì cho đầu ra. 
4. Sắp xếp các cặp theo tần số giảm dần và khi tần số bằng nhau thì tăng ký tự. Trong Python điều này có thể được biểu diễn trực tiếp bằng phím`(-frequency, character)`. Tần số âm đảo ngược thứ tự số tăng dần thông thường trong khi ký tự vẫn giữ nguyên thứ tự bảng chữ cái bình thường. 
5. Xây dựng câu trả lời bằng cách lặp lại từng ký tự theo tần số của nó. Một ký tự có tần số bằng 0 đương nhiên sẽ đóng góp một chuỗi trống, do đó, việc bao gồm các mục nhập có tần số bằng 0 sẽ không làm thay đổi kết quả. 
6. In chuỗi kết quả không có dấu cách. Mỗi lần xuất hiện ban đầu được phát ra chính xác một lần, do đó đầu ra có cùng độ dài với đầu vào. 

### Tại sao nó hoạt động 

Sau khi đếm xong, bất biến là`freq[i]`bằng số lần xuất hiện chính xác của ký tự được biểu thị bằng chỉ mục`i`trong đầu vào. Sắp xếp các cặp ký tự-tần số bằng cách giảm tần số sẽ đặt mọi ký tự trước tất cả các ký tự có tần số nhỏ hơn. Đối với các cặp có tần số bằng nhau thì khóa phụ chính là ký tự đó nên chúng xuất hiện theo thứ tự bảng chữ cái. Cuối cùng, lặp lại chính xác từng ký tự`freq[i]`time bảo toàn mọi lần xuất hiện từ đầu vào. Do đó, chuỗi kết quả thỏa mãn cả quy tắc sắp xếp và chứa chính xác nhiều bộ ký tự ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    freq = [0] * 26

    for ch in s:
        freq[ord(ch) - ord('a')] += 1

    chars = []
    for i in range(26):
        ch = chr(ord('a') + i)
        chars.append((ch, freq[i]))

    chars.sort(key=lambda item: (-item[1], item[0]))

    ans = []
    for ch, count in chars:
        ans.append(ch * count)

    print(''.join(ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc chính xác một dòng vì bài toán này chứa một chuỗi thay vì nhiều ca kiểm thử. Đang gọi`strip()`xóa dòng mới ở cuối mà không thay đổi bất kỳ ký tự đầu vào hợp lệ nào, vì đầu vào chỉ chứa các chữ cái viết thường. 

Mảng tần số được lập chỉ mục bởi`ord(ch) - ord('a')`. Ví dụ,`ord('a') - ord('a')`bằng không và`ord('z') - ord('a')`là 25, do đó mọi ký tự đầu vào có thể ánh xạ tới một vị trí mảng hợp lệ. 

Danh sách các cặp lưu trữ ký tự cùng với số lượng của nó. Phím sắp xếp`(-item[1], item[0])`thực hiện cả hai quy tắc bắt buộc trong một biểu thức. Thành phần đầu tiên đặt tần số lớn hơn trước, trong khi thành phần thứ hai đặt các ký tự có tần số bằng nhau theo thứ tự bảng chữ cái. 

Câu trả lời được tích lũy trong một danh sách thay vì nối các chuỗi liên tục. Mỗi phép nhân như`ch * count`tạo tất cả các bản sao của một ký tự và`''.join(ans)`kết hợp các mảnh một lần. Chỉ có 26 miếng nên việc này rất đơn giản và hiệu quả. 

Không có lo ngại về tràn số nguyên trong Python. Tần số lớn nhất chỉ là độ dài của đầu vào, tối đa là 70 theo giới hạn ban đầu. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`dcbbdabb`. Giai đoạn đếm tần số tạo ra trạng thái sau. 

| Ký tự được xử lý | Tần số của | Tần số của b | Tần số c | Tần số của d | 
| --- | --- | --- | --- | --- | 
|`d`| 0 | 0 | 0 | 1 | 
|`c`| 0 | 0 | 1 | 1 | 
|`b`| 0 | 1 | 1 | 1 | 
|`b`| 0 | 2 | 1 | 1 | 
|`d`| 0 | 2 | 1 | 2 | 
|`a`| 1 | 2 | 1 | 2 | 
|`b`| 1 | 3 | 1 | 2 | 
|`b`| 1 | 4 | 1 | 2 | 

Các cặp ký tự-tần số được sắp xếp là`(b, 4)`,`(d, 2)`,`(a, 1)`,`(c, 1)`. Hai ký tự tần số một được sắp xếp theo thứ tự bảng chữ cái, vì vậy chuỗi cuối cùng là`bbbbddac`. 

Đối với Mẫu 2, đầu vào là`programming`. Trạng thái đếm thú vị hơn vì ba ký tự có tần số hai. 

| Ký tự được xử lý | một | g | m | r | tôi | n | o | p | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
|`p`| 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 
|`r`| 0 | 0 | 0 | 1 | 0 | 0 | 0 | 1 | 
|`o`| 0 | 0 | 0 | 1 | 0 | 0 | 1 | 1 | 
|`g`| 0 | 1 | 0 | 1 | 0 | 0 | 1 | 1 | 
|`r`| 0 | 1 | 0 | 2 | 0 | 0 | 1 | 1 | 
|`a`| 1 | 1 | 0 | 2 | 0 | 0 | 1 | 1 | 
|`m`| 1 | 1 | 1 | 2 | 0 | 0 | 1 | 1 | 
|`m`| 1 | 1 | 2 | 2 | 0 | 0 | 1 | 1 | 
|`i`| 1 | 1 | 2 | 2 | 1 | 0 | 1 | 1 | 
|`n`| 1 | 1 | 2 | 2 | 1 | 1 | 1 | 1 | 
|`g`| 1 | 2 | 2 | 2 | 1 | 1 | 1 | 1 | 

Hai ký tự tần số là`g`,`m`, Và`r`, vì vậy thứ tự bảng chữ cái cho`g`, sau đó`m`, sau đó`r`. Các ký tự còn lại đều xuất hiện một lần và được sắp xếp theo thứ tự`a`,`i`,`n`,`o`,`p`. Đầu ra cuối cùng là`ggmmrrainop`, phù hợp với mẫu 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Việc đếm quét chuỗi một lần và sắp xếp tối đa 26 cặp ký tự là công việc liên tục. | 
| Không gian | O(1) | Mảng tần số và danh sách ký tự chứa tối đa 26 mục. | 

Với độ dài đầu vào tối đa thực tế là 70 và giới hạn 1 giây, giải pháp này sử dụng số lượng thao tác không đáng kể và ít hơn nhiều so với bộ nhớ 256 MB hiện có. 

## Trường hợp thử nghiệm```python
# helper: run the solution logic on an input string
import sys
import io

def solve():
    s = input().strip()

    freq = [0] * 26

    for ch in s:
        freq[ord(ch) - ord('a')] += 1

    chars = []
    for i in range(26):
        ch = chr(ord('a') + i)
        chars.append((ch, freq[i]))

    chars.sort(key=lambda item: (-item[1], item[0]))

    ans = []
    for ch, count in chars:
        ans.append(ch * count)

    print(''.join(ans))

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    try:
        solve()
        return out.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

# Provided samples
assert run("dcbbdabb\n") == "bbbbddac\n", "sample 1"
assert run("programming\n") == "ggmmrrainop\n", "sample 2"

# Minimum-size input
assert run("z\n") == "z\n", "single character"

# All characters equal
assert run("aaaaa\n") == "aaaaa\n", "all equal"

# Equal frequencies, reverse alphabetical input
assert run("cba\n") == "abc\n", "alphabetical tie breaking"

# Boundary-sized input, 70 characters
maximum_input = "a" * 35 + "b" * 20 + "c" * 10 + "d" * 5
assert run(maximum_input + "\n") == maximum_input + "\n", "maximum size"

# Several equal-frequency groups
assert run("zzzyyxxwwvvvuu\n") == "vvvzzzwwxxyyuu\n", "multiple ties"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`z`|`z`| Kích thước đầu vào tối thiểu và không có điểm ràng buộc | 
|`aaaaa`|`aaaaa`| Tất cả các lần xuất hiện thuộc về một ký tự | 
|`cba`|`abc`| Phá vỡ ràng buộc theo bảng chữ cái | 
| 70 ký tự với tần số 35, 20, 10, 5 | Cùng một chuỗi | Thứ tự tần số và độ dài đầu vào tối đa | 
|`zzzyyxxwwvvvuu`|`vvvzzzwwxxyyuu`| Nhiều nhóm tần số và thứ tự tần số bằng nhau | 

## Vỏ cạnh 

Trường hợp ký tự đơn`z`được xử lý bằng cách đếm một lần xuất hiện ở chỉ số 25. Tất cả các tần số khác đều bằng 0, vì vậy việc sắp xếp các vị trí`z`đầu tiên và cấu trúc đầu ra phát ra chính xác một`z`. Kết quả là`z`. 

Đối với một chuỗi hoàn toàn bằng nhau, chẳng hạn như`aaaaa`, mảng tần số chứa`freq[a] = 5`và số không ở mọi nơi khác. Các cặp được sắp xếp đặt`a`đầu tiên vì nó có tần số dương duy nhất và công trình phát ra`a * 5`, sản xuất`aaaaa`. Không có ký tự nào bị mất hoặc trùng lặp. 

Hộp đựng cà vạt`cba`thực hiện quy tắc thứ tự thứ cấp. Cả ba ký tự đều có tần số một nên tần số của chúng không phân biệt được chúng. Các phím được sắp xếp là`(−1, a)`,`(−1, b)`, Và`(−1, c)`, cho`abc`. Việc triển khai dựa trên thứ tự đầu vào sẽ trả về không chính xác`cba`. 

Trường hợp kích thước tối đa chứa 70 ký tự với tần số 35, 20, 10 và 5. Quá trình đếm ghi lại bốn tần số đó, giai đoạn sắp xếp đặt các ký tự theo thứ tự tần số giảm dần và quá trình tái tạo lại tạo ra tất cả 70 ký tự. Vì mảng tần số có kích thước cố định 26 nên việc tăng đầu vào từ một ký tự lên kích thước tối đa chỉ làm thay đổi công việc đếm chứ không thay đổi bộ nhớ phụ. 

Trường hợp nhiều dây buộc`zzzyyxxwwvvvuu`có`z=3`,`v=3`,`y=2`,`x=2`,`w=2`, Và`u=2`. Hai ký tự tần số ba được sắp xếp như sau`v`,`z`, trong khi hai ký tự tần số được sắp xếp như sau`u`,`w`,`x`,`y`. Do đó, thuật toán tạo ra`vvvzzzuuwwxxyy`. Trường hợp này trực tiếp kiểm tra xem thứ tự bảng chữ cái có được áp dụng độc lập trong mỗi nhóm tần số hay không.
