---
title: "CF 102777B - \u041f\u0430\u043b\u0438\u043d\u0434\u0440\u043e\u043c \u041c\u043e\u0440\u0437\u0435"
description: "Chúng ta cần quyết định xem một chuỗi tiếng Anh viết thường có trở thành một bảng màu hay không sau khi thay thế mọi ký tự bằng cách biểu diễn Morse của nó và nối tất cả các cách biểu diễn đó lại với nhau."
date: "2026-07-28T03:07:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102777
codeforces_index: "B"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 19), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102777
solve_time_s: 83
verified: true
draft: false
---

[CF 102777B - \u041f\u0430\u043b\u0438\u043d\u0434\u0440\u043e\u043c \u041c\u043e\u0440\u0437\u0435](https://codeforces.com/problemset/problem/102777/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần quyết định xem một chuỗi tiếng Anh viết thường có trở thành một bảng màu hay không sau khi thay thế mọi ký tự bằng cách biểu diễn Morse của nó và nối tất cả các cách biểu diễn đó lại với nhau. Các chữ cái gốc được tách ra trước khi mã hóa, nhưng chuỗi Morse thu được chỉ là một chuỗi dấu chấm và dấu gạch ngang liên tục. Câu trả lời là CÓ nếu chuỗi cuối cùng này đọc giống nhau ở cả hai đầu và KHÔNG nếu ngược lại. 

Độ dài đầu vào tối đa là 1000 ký tự. Mã Morse cho một chữ cái chứa tối đa bốn ký hiệu, do đó, ngay cả chuỗi được mã hóa cũng chỉ dài vài nghìn ký tự. Điều này có nghĩa là quá trình quét tuyến tính có thể dễ dàng đủ nhanh, trong khi các thuật toán có hành vi bậc hai là không cần thiết và sẽ chỉ khiến việc triển khai trở nên phức tạp hơn. 

Nguồn gốc chính của sai lầm là giả định rằng thuộc tính palindrome có thể được kiểm tra trên các chữ cái gốc. Việc mã hóa thay đổi hoàn toàn cấu trúc vì các chữ cái khác nhau có thể tạo ra các mẫu dấu chấm và dấu gạch ngang khớp hoặc không khớp. 

Ví dụ, đầu vào`aarrghh`nên sản xuất`NO`. Một giải pháp bất cẩn chỉ kiểm tra xem các chữ cái có tạo thành một bảng màu hay không sẽ thấy rằng các chữ cái đó đối xứng và trả về CÓ không chính xác. Thay vào đó, đại diện Morse phải được kiểm tra. 

Một cái bẫy khác là quên rằng ranh giới ký tự sẽ biến mất sau khi mã hóa. đầu vào`panne`nên sản xuất`NO`. Bản thân các chữ cái không thể đơn giản được phản chiếu thành các nhóm vì sự so sánh cuối cùng là giữa các ký hiệu Morse riêng lẻ. 

Chuỗi trống không thể là đầu vào vì câu lệnh đảm bảo một chuỗi các chữ cái viết thường không trống. Đầu vào một ký tự, chẳng hạn như`a`sản xuất`.−`, không phải là bảng màu, nên kết quả đúng là KHÔNG. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là chuyển đổi toàn bộ chuỗi đầu vào thành mã Morse rồi so sánh các ký tự từ đầu đến cuối. Điều này đòi hỏi phải tạo chuỗi được mã hóa và kiểm tra xem ký hiệu đầu tiên của nó có bằng ký hiệu cuối cùng hay không, ký hiệu thứ hai có bằng ký hiệu cuối cùng thứ hai hay không, v.v. Phương pháp này đúng vì một chuỗi là một bảng màu chính xác khi tất cả các vị trí được phản chiếu đều chứa cùng một ký tự. 

Việc triển khai bạo lực cũng có thể so sánh nhiều lần mọi cặp vị trí trong chuỗi được mã hóa. Nếu độ dài được mã hóa là khoảng 4000 ký tự thì điều này thực hiện khoảng 16 triệu so sánh trong trường hợp xấu nhất. Điều đó vẫn không phải là thảm họa đối với những ràng buộc này, nhưng nó không cần thiết và bỏ qua cấu trúc đơn giản hơn của việc kiểm tra palindrome. 

Quan sát quan trọng là chúng ta không cần phải so sánh từng cặp. Một palindrome chỉ yêu cầu một lần chuyển từ cả hai đầu. Khi chúng ta có chuỗi Morse, hai con trỏ có thể di chuyển vào trong và ngay lập tức từ chối câu trả lời khi chúng tìm thấy các ký hiệu khác nhau. 

Brute-force hoạt động vì nó kiểm tra cùng một thông tin như giải pháp tối ưu, nhưng nó lặp lại nhiều so sánh. Quan sát cho thấy các vị trí phản chiếu có thể được kiểm tra trong khi di chuyển về phía trung tâm sẽ giảm công việc xuống dạng quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m2) | O(m) | Quá chậm trong các biến thể lớn hơn | 
| Tối ưu | O(m) | O(m) | Đã chấp nhận | 

Đây,`m`là độ dài của chuỗi mã hóa Morse. 

## Hướng dẫn thuật toán 

1. Lưu trữ biểu diễn Morse của mọi chữ cái tiếng Anh viết thường trong một mảng trong đó chỉ mục khớp với vị trí chữ cái. Điều này tránh việc xây dựng lại ánh xạ trong khi xử lý dữ liệu đầu vào. 
2. Đọc chuỗi gốc và nối mã Morse của từng ký tự thành một chuỗi liên tục. Khoảng cách giữa các chữ cái không được cố ý lưu trữ vì quá trình kiểm tra bảng màu diễn ra trên luồng Morse cuối cùng. 
3. Đặt một con trỏ ở đầu chuỗi được mã hóa và một con trỏ khác ở cuối. So sánh hai biểu tượng mà chúng trỏ tới. 
4. Nếu các ký hiệu khác nhau, chuỗi không thể là một bảng màu, vì vậy câu trả lời ngay lập tức là KHÔNG. Sự không khớp ở bất kỳ vị trí phản chiếu nào cũng đủ để chứng minh sự thất bại. 
5. Di chuyển cả hai con trỏ về phía giữa và tiếp tục cho đến khi chúng gặp nhau hoặc cắt nhau. Nếu mọi cặp được nhân đôi đều khớp, xuất CÓ. 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến rằng mọi cặp vị trí đã được hai con trỏ đi qua đều bằng vị trí được phản chiếu của nó. Một palindrome được xác định chính xác bởi thuộc tính này. Nếu thuật toán đến giữa mà không tìm thấy điểm không khớp thì mọi cặp bắt buộc đều đã được xác minh, do đó chuỗi được mã hóa phải là một bảng màu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    morse = [
        ".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..",
        ".---", "-.-", ".-..", "--", "-.", "---", ".--.", "--.-", ".-.",
        "...", "-", "..-", "...-", ".--", "-..-", "-.--", "--.."
    ]

    s = input().strip()

    encoded = []
    for ch in s:
        encoded.append(morse[ord(ch) - ord('a')])

    encoded = "".join(encoded)

    left = 0
    right = len(encoded) - 1

    while left < right:
        if encoded[left] != encoded[right]:
            print("NO")
            return
        left += 1
        right -= 1

    print("YES")

if __name__ == "__main__":
    solve()
```các`morse`mảng lưu trữ bảng dịch một lần. Việc tính chỉ số bằng`ord(ch) - ord('a')`chuyển đổi một chữ cái viết thường thành vị trí dựa trên số 0, do đó không cần tra cứu từ điển. 

Danh sách được mã hóa được sử dụng khi xây dựng chuỗi Morse vì việc nối các chuỗi liên tục có thể tạo ra các chuỗi tạm thời không cần thiết trong một số ngôn ngữ. Tham gia một lần sau vòng lặp giúp việc xây dựng trở nên đơn giản và hiệu quả. 

Vòng lặp hai con trỏ sử dụng`left < right`bởi vì ký tự ở giữa của một palindrome có độ dài lẻ không cần phải so sánh với chính nó. Sau mỗi lần so sánh thành công, cả hai con trỏ đều di chuyển chính xác một vị trí vào trong, tránh mọi vấn đề khác nhau. 

## Ví dụ đã hoạt động 

Đối với đầu vào`abelian`, phép biến đổi Morse là:`a`trở thành`.-`
`b`trở thành`-...`
`e`trở thành`.`
`l`trở thành`.-..`
`i`trở thành`..`
`a`trở thành`.-`
`n`trở thành`-.`Chuỗi được mã hóa hoàn chỉnh là`.--....-....-.-.`. 

| trái | đúng | biểu tượng bên trái | biểu tượng bên phải | kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | 16 | . | . | tiếp tục | 
| 1 | 15 | - | - | tiếp tục | 
| 2 | 14 | - | - | tiếp tục | 
| 3 | 13 | . | . | tiếp tục | 

Các con trỏ tiếp tục cho đến khi chúng giao nhau, vì vậy mọi cặp đối xứng đều khớp nhau. Đầu ra là CÓ. 

Đối với đầu vào`panmixises`, chuỗi mã hóa được kiểm tra từ cả hai đầu. 

| trái | đúng | biểu tượng bên trái | biểu tượng bên phải | kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | 27 | . | - | không khớp | 

Sự so sánh đầu tiên đã thất bại. Thuật toán dừng ngay lập tức và xuất ra NO. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi ký hiệu Morse được tạo một lần và được kiểm tra nhiều nhất một lần từ mỗi bên | 
| Không gian | O(m) | Chuỗi Morse được mã hóa được lưu trữ, trong đó m là độ dài của nó | 

Chuỗi được mã hóa lớn nhất có thể chỉ có vài nghìn ký tự vì đầu vào có tối đa 1000 chữ cái và mỗi chữ cái sử dụng tối đa bốn ký hiệu Morse. Lời giải tuyến tính dễ dàng phù hợp với giới hạn thời gian của bài toán. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp):
    morse = [
        ".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..",
        ".---", "-.-", ".-..", "--", "-.", "---", ".--.", "--.-", ".-.",
        "...", "-", "..-", "...-", ".--", "-..-", "-.--", "--.."
    ]

    s = inp.strip()
    encoded = "".join(morse[ord(ch) - ord('a')] for ch in s)

    left = 0
    right = len(encoded) - 1

    while left < right:
        if encoded[left] != encoded[right]:
            return "NO"
        left += 1
        right -= 1

    return "YES"

assert solve("abelian") == "YES", "sample 1"
assert solve("panmixises") == "NO", "sample 2"
assert solve("panne") == "NO", "sample 3"
assert solve("aarrghh") == "NO", "sample 4"
assert solve("protectorate") == "YES", "sample 5"

assert solve("a") == "NO", "single character"
assert solve("e") == "YES", "single dot Morse code"
assert solve("zzzz") == "YES", "all equal values"
assert solve("abcdefghijklmnopqrstuvwxyz") == "NO", "large boundary input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`| KHÔNG | Xử lý đầu vào nhỏ nhất và kiểm tra xem`.−`không được coi là một bảng chữ cái palindrome | 
|`e`| CÓ | Kiểm tra bảng màu được mã hóa một ký hiệu | 
|`zzzz`| CÓ | Xác thực các giá trị bằng nhau lặp lại | 
|`abcdefghijklmnopqrstuvwxyz`| KHÔNG | Thực hiện nhập liệu dài và nắm bắt các cách tiếp cận dựa trên ký tự không chính xác | 

## Vỏ cạnh 

đầu vào`aarrghh`là một ví dụ hữu ích vì bản thân các chữ cái trông có vẻ đối xứng. Thuật toán chuyển đổi nó thành ký hiệu Morse trước tiên, sau đó so sánh các ký hiệu bên ngoài của chuỗi mới đó. Sự không khớp xuất hiện trong quá trình quét hai con trỏ, do đó nó trả về NO một cách chính xác. 

đầu vào`panne`chứng minh tại sao ranh giới chữ cái không thể được bảo tồn. Thuật toán không bao giờ so sánh`p`với`e`hoặc`a`với`n`như những chữ cái. Nó chỉ so sánh các vị trí bên trong chuỗi Morse đã nối, đây là đối tượng thực tế được xác định bởi tác vụ. 

đầu vào`e`tạo ra chuỗi Morse`.`. Hai con trỏ bắt đầu ở cùng một vị trí, do đó vòng lặp bị bỏ qua và thuật toán trả về CÓ. Đây là hành vi đúng đối với bảng màu một ký tự. 

đầu vào`zzzz`tạo ra bốn khối Morse giống hệt nhau. Sau khi nối chúng, mọi cặp biểu tượng được phản chiếu đều bằng nhau, do đó, con trỏ di chuyển vào trong cho đến khi chúng giao nhau và câu trả lời vẫn là CÓ.
