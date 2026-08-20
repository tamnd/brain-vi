---
title: "CF 102168D - \u0411\u0435\u0437 \u043e\u0434\u043d\u043e\u0433\u043e \u0441\u0438\u043c\u0432\u043e\u043b\u0430"
description: "Chúng ta có một chuỗi s có độ dài n. Đối với mọi vị trí i, Vasya viết chuỗi thu được bằng cách loại bỏ chính xác ký tự ở vị trí i. Tất cả các chuỗi kết quả đều có độ dài n - 1 và nhiệm vụ là đếm xem có bao nhiêu trong số n chuỗi này thực sự khác nhau."
date: "2026-08-19T07:19:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102168
codeforces_index: "D"
codeforces_contest_name: "\u041b\u0438\u0447\u043d\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u043e\u0433\u043e \u0443\u043d\u0438\u0432\u0435\u0440\u0441\u0438\u0442\u0435\u0442\u0430 \u0441\u0440\u0435\u0434\u0438 \u043d\u043e\u0432\u0438\u0447\u043a\u043e\u0432 2018-2019"
rating: 0
weight: 102168
solve_time_s: 85
verified: true
draft: false
---

[CF 102168D - \u0411\u0435\u0437 \u043e\u0434\u043d\u043e\u0433\u043e \u0441\u0438\u043c\u0432\u043e\u043b\u0430](https://codeforces.com/problemset/problem/102168/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi`s`chiều dài`n`. Đối với mọi vị trí`i`, Vasya viết chuỗi thu được bằng cách loại bỏ chính xác ký tự tại vị trí`i`. Tất cả các chuỗi kết quả đều có độ dài`n - 1`, và nhiệm vụ là đếm xem có bao nhiêu trong số này`n`các chuỗi thực sự khác nhau. 

Đầu vào chứa một chuỗi chữ Latinh viết thường. Độ dài của nó nằm trong khoảng từ 2 đến 200.000, do đó, một thuật toán thực hiện công việc tỷ lệ với`n^2`là quá đắt. Với 200.000 vị trí, ngay cả một vòng lặp bậc hai đơn giản cũng sẽ cần khoảng 40 tỷ lần lặp trong trường hợp xấu nhất, không thể đáp ứng giới hạn 2 giây. Chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. 

Phần tinh tế là hai vị trí bị loại bỏ khác nhau có thể tạo ra cùng một chuỗi. Ví dụ, đối với`aaa`, việc xóa bất kỳ ký tự nào trong ba ký tự sẽ mang lại`aa`, vì vậy câu trả lời là 1 chứ không phải 3. Một giải pháp bất cẩn chỉ tính các vị trí xóa sẽ thất bại trong trường hợp này. 

Một trường hợp ranh giới khác là`ab`. Loại bỏ ký tự đầu tiên sẽ mang lại`b`, trong khi loại bỏ cái thứ hai`a`, vì vậy câu trả lời là 2. Việc triển khai vô tình coi mọi cặp vị trí liền kề là tương đương sẽ trả về 1 không chính xác. 

Một ví dụ dài hơn là`aabb`. Xóa một trong hai ký tự khỏi ký tự đầu tiên`aa`cho`abb`, trong khi xóa một trong hai ký tự khỏi cuối cùng`bb`cho`aab`. Câu trả lời là 2. Sự bằng nhau của các ký tự quan trọng trong toàn bộ khoảng thời gian giữa hai vị trí xóa, không chỉ ở hai điểm cuối. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng chuỗi kết quả cho mọi vị trí có thể bị xóa và đặt tất cả các chuỗi kết quả vào một tập hợp. Điều này đúng vì mỗi chuỗi được tạo đại diện cho chính xác một ngày của Vasya và tập hợp này sẽ loại bỏ các chuỗi trùng lặp. Tuy nhiên, việc xây dựng một kết quả tốn kém`O(n)`, và có`n`có thể xóa, đưa ra`O(n^2)`thời gian. Vì`n = 200000`, tức là khoảng 40 tỷ thao tác ký tự trước khi tính đến chuỗi và chi phí thiết lập. 

Quan sát hữu ích là chúng ta thực sự không cần xây dựng bất kỳ chuỗi nào trong số đó. Hãy xem xét hai vị trí xóa`i < j`. Trước vị trí`i`, cả hai chuỗi kết quả đều chứa chính xác cùng một tiền tố. Sau vị trí`j`, cả hai đều chứa chính xác cùng một hậu tố. Phần có khả năng khác nhau duy nhất là khoảng thời gian từ`i`bởi vì`j`. 

Đang xóa`i`lá```
s[i+1] s[i+2] ... s[j]
```từ khoảng thời gian đó, trong khi xóa`j`lá```
s[i] s[i+1] ... s[j-1]
```Để hai chuỗi này bằng nhau, mỗi ký tự trong khoảng`s[i..j]`phải giống nhau. Nếu thậm chí một ký tự khác nhau thì hai chuỗi kết quả sẽ khác nhau ở vị trí tương ứng. 

Điều này đưa ra một quy tắc tương đương rất đơn giản: xóa vị trí`i`Và`j`tạo ra cùng một chuỗi chính xác khi mọi ký tự giữa chúng đều bằng nhau. Nói cách khác, tất cả các vị trí thuộc một chuỗi ký tự tối đa bằng nhau sẽ tạo ra kết quả giống nhau khi một ký tự trong chuỗi đó bị xóa. 

Vì`aabb`, số lần chạy là`aa`Và`bb`, do đó có hai chuỗi kết quả riêng biệt. Vì`abca`, mỗi ký tự hình thành cách chạy riêng nên có bốn kết quả riêng biệt. Vì`zzz`, chỉ có một lần chạy nên mỗi lần xóa đều tạo ra cùng một chuỗi. 

Do đó, câu trả lời chỉ đơn giản là số lần chạy liên tiếp tối đa của các ký tự bằng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n²) trong trường hợp xấu nhất | Quá chậm | 
| Đếm số lần chạy có ký tự bằng nhau | O(n) | O(1) không gian phụ trợ | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi`s`và bắt đầu câu trả lời từ 1. Vì`n >= 2`, tồn tại ít nhất một kết quả xóa và ký tự đầu tiên bắt đầu lần chạy đầu tiên. 
2. Quét chuỗi từ ký tự thứ hai đến cuối. Bất cứ khi nào`s[i]`khác với`s[i - 1]`, một lần chạy tối đa mới sẽ bắt đầu, vì vậy hãy tăng câu trả lời. 
3. Bỏ qua những vị trí có`s[i] == s[i - 1]`. Chúng thuộc cùng một lần chạy và việc xóa bất kỳ ký tự nào khỏi lần chạy đó sẽ mang lại cùng một chuỗi kết quả. 
4. In số lần chạy. 

### Tại sao nó hoạt động 

Phân chia chuỗi thành các chuỗi tối đa có ký tự bằng nhau. Giả sử hai vị trí xóa thuộc cùng một lần chạy. Mọi ký tự giữa chúng đều giống hệt nhau, do đó việc chuyển việc xóa từ vị trí này sang vị trí khác không làm thay đổi gì trong chuỗi kết quả. Do đó tất cả các vị trí trong một lần chạy đều tương ứng với một kết quả riêng biệt. 

Bây giờ giả sử hai vị trí thuộc về các lần chạy khác nhau. Giữa chúng có một ranh giới nơi hai ký tự liên tiếp khác nhau. Hai kết quả xóa khác nhau ở ranh giới đó vì một kết quả làm dịch chuyển các ký tự ở một bên của ranh giới trong khi kết quả kia thì không. Do đó, các vị trí từ các lần chạy khác nhau không thể tạo ra cùng một chuỗi. 

Vì vậy, có chính xác một chuỗi kết quả riêng biệt cho mỗi lần chạy ký tự bằng nhau tối đa và việc đếm các lần chạy đó sẽ đưa ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_string(s: str) -> int:
    answer = 1

    for i in range(1, len(s)):
        if s[i] != s[i - 1]:
            answer += 1

    return answer

def main() -> None:
    s = input().strip()
    print(solve_string(s))

if __name__ == "__main__":
    main()
```các`solve_string`hàm thực hiện trực tiếp thuật toán đếm số lần chạy. Giá trị ban đầu`answer = 1`hợp lệ vì độ dài đầu vào ít nhất là 2, do đó chuỗi luôn có ít nhất một lần chạy. 

Vòng lặp bắt đầu ở chỉ mục 1 vì chỉ mục 0 không có ký tự trước đó để so sánh. Mỗi thay đổi từ`s[i - 1]`ĐẾN`s[i]`bắt đầu một lần chạy mới và đóng góp chính xác một kết quả xóa riêng biệt mới. 

Không cần phải xây dựng các chuỗi thu được sau khi xóa và không cần thiết lập. Số nguyên Python cũng là quá đủ cho câu trả lời này vì nhiều nhất là`n`, chỉ có 200.000. 

các`strip()`cuộc gọi loại bỏ dòng mới được cung cấp bởi đầu vào tiêu chuẩn. Vì chuỗi chỉ chứa các chữ cái Latinh viết thường nên không có khoảng trắng có ý nghĩa nào có thể bị xóa do vô tình. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên,`abca`, mỗi cặp lân cận đều khác nhau. 

| chỉ mục`i`| Nhân vật | Ký tự trước | Chạy mới? | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 |`a`| không | vâng, lần chạy đầu tiên | 1 | 
| 1 |`b`|`a`| vâng | 2 | 
| 2 |`c`|`b`| vâng | 3 | 
| 3 |`a`|`c`| vâng | 4 | 

Bốn lần chạy là`a`,`b`,`c`, Và`a`. Xóa từng vị trí sẽ cho kết quả khác nhau nên đáp án là 4. 

Đối với mẫu thứ hai,`zzz`, tất cả các ký tự thuộc về một lần chạy. 

| chỉ mục`i`| Nhân vật | Ký tự trước | Chạy mới? | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 |`z`| không | vâng, lần chạy đầu tiên | 1 | 
| 1 |`z`|`z`| không | 1 | 
| 2 |`z`|`z`| không | 1 | 

Xóa bất kỳ cái nào`z`lá`zz`, do đó chỉ có một chuỗi riêng biệt. Thuật toán trả về đúng 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được kiểm tra chính xác một lần. | 
| Không gian | O(1) | Chỉ các biến câu trả lời và vòng lặp được lưu trữ ngoài chuỗi đầu vào. | 

Với`n <= 200000`, thuật toán chỉ thực hiện khoảng 200.000 so sánh ký tự. Điều này thoải mái trong giới hạn thời gian 2 giây đã nêu và sử dụng bộ nhớ phụ không đáng kể. 

## Trường hợp thử nghiệm```
# helper: run solution on input string, return output string
def run(inp: str) -> str:
    s = inp.strip()
    return str(solve_string(s))

# Provided samples
assert run("abca") == "4", "sample 1"
assert run("zzz") == "1", "sample 2"

# Minimum-size inputs
assert run("aa") == "1", "two equal characters form one run"
assert run("ab") == "2", "two different characters form two runs"

# All characters equal
assert run("aaaaaaaaaa") == "1", "all deletions produce the same string"

# Every character starts a new run
assert run("abababab") == "8", "alternating characters produce eight runs"

# Several runs with different lengths
assert run("aabbbaa") == "3", "runs are aa, bbb, aa"

# Maximum-size input, all equal
maximum_equal = "a" * 200000
assert run(maximum_equal) == "1", "maximum length with one run"

# Maximum-size input, alternating characters
maximum_alternating = "ab" * 100000
assert run(maximum_alternating) == "200000", "maximum length with every position in its own run"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`aa`| 1 | Độ dài tối thiểu và kết quả xóa trùng lặp | 
|`ab`| 2 | Độ dài tối thiểu với hai lần chạy riêng biệt | 
|`aaaaaaaaaa`| 1 | Tất cả các vị trí xóa đều tương đương | 
|`abababab`| 8 | Mỗi vị trí tạo thành một cuộc chạy riêng | 
|`aabbbaa`| 3 | Nhiều lần chạy với độ dài khác nhau | 
|`a * 200000`| 1 | Kích thước đầu vào tối đa và trạng thái quét kích thước không đổi | 
|`(ab) * 100000`| 200000 | Số lần chạy và xử lý ranh giới tối đa | 

## Vỏ cạnh 

cho`aa`, thuật toán bắt đầu bằng`answer = 1`. Tại chỉ số 1, ký tự hiện tại cũng`a`, vì vậy câu trả lời vẫn là 1. Cả hai khả năng xóa đều tạo ra`a`, xác nhận việc xử lý trường hợp ký tự bằng kích thước tối thiểu. 

Vì`ab`, câu trả lời ban đầu là 1 và so sánh ở chỉ số 1 tìm thấy`b != a`, tăng đáp số lên 2. Hai kết quả xóa là`b`Và`a`, vì vậy chúng thực sự khác nhau. 

Vì`aaaa`, quá trình quét không bao giờ thấy ký tự nào khác với ký tự trước đó. Câu trả lời vẫn là 1, phù hợp với thực tế là việc xóa bất kỳ vị trí nào trong bốn vị trí sẽ tạo ra`aaa`. 

Vì`aabbbaa`, quá trình quét sẽ thấy các chuyển tiếp`a -> b`Và`b -> a`. Bắt đầu từ 1, hai lần chuyển đổi này tạo ra câu trả lời là 3. Các lần chạy là`aa`,`bbb`, Và`aa`và mỗi lần chạy tương ứng với chính xác một chuỗi riêng biệt. 

Đối với chuỗi xen kẽ có độ dài tối đa`abab...ab`, mỗi cặp lân cận đều khác nhau. Câu trả lời được tăng lên ở mọi vị trí sau vị trí đầu tiên, tạo ra 200.000. Việc này kiểm tra cả giới hạn trên của câu trả lời và việc không có lỗi sai sót nào trong vòng lặp. 

Trường hợp cạnh chính đằng sau toàn bộ giải pháp là một cặp vị trí xóa bên trong cùng một chuỗi ký tự bằng nhau. Vì`aabb`, xóa một trong hai ký tự đầu tiên`aa`sản xuất`abb`. Thuật toán chỉ tính toàn bộ lần chạy đó một lần. Khi chuyển từ lần chạy đầu tiên sang lần chạy thứ hai, nhân vật sẽ thay đổi từ`a`ĐẾN`b`, do đó, câu trả lời tăng lên 2, khớp chính xác với hai chuỗi kết quả riêng biệt.
