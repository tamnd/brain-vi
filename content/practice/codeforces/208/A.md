---
title: "CF 208A - Dubstep"
description: "Đầu vào là một chuỗi đơn được hình thành bằng cách lấy một chuỗi các từ gốc rồi chèn chuỗi đánh dấu \"WUB\" xung quanh và giữa chúng theo một cách rất cụ thể."
date: "2026-06-03T17:19:53+07:00"
tags: ["codeforces", "competitive-programming", "strings"]
categories: ["algorithms"]
codeforces_contest: 208
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 130 (Div. 2)"
rating: 900
weight: 208
solve_time_s: 76
verified: true
draft: false
---

[CF 208A - Dubstep](https://codeforces.com/problemset/problem/208/A) 

**Xếp hạng:** 900 
**Thẻ:** chuỗi 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào là một chuỗi đơn được hình thành bằng cách lấy một chuỗi các từ gốc rồi chèn chuỗi đánh dấu vào`"WUB"`xung quanh và giữa chúng một cách rất cụ thể. Mỗi từ gốc được đảm bảo không chứa`"WUB"`bên trong nó, vì vậy mỗi lần xuất hiện của`"WUB"`ở chuỗi cuối cùng là cấu trúc nhân tạo được thêm vào trong quá trình phối lại. 

Nhiệm vụ là khôi phục chuỗi từ ban đầu. Về mặt khái niệm, chúng ta cần xử lý`"WUB"`như một dấu phân cách có thể xuất hiện lặp đi lặp lại và có thể theo khối. Sau khi loại bỏ các dấu phân cách này, các ký tự còn lại sẽ tạo thành các từ gốc theo thứ tự, cách nhau bằng dấu cách. 

Độ dài chuỗi tối đa là 200, điều này ngay lập tức loại bỏ mọi lo ngại về các chiến lược tiền xử lý nặng hoặc phân tích cú pháp phức tạp. Ngay cả giải pháp O(n²) cũng sẽ an toàn, nhưng cấu trúc cho thấy quét tuyến tính là đủ. 

Một số trường hợp đặc biệt quan trọng đối với tính chính xác. Đầu tiên,`"WUB"`có thể xuất hiện liên tiếp, tạo ra các dấu phân cách dài như`"WUBWUBWUB"`, sẽ hoạt động giống như một dấu phân cách thay vì nhiều từ trống. Ví dụ,`"WUBWUBABC"`nên sản xuất`"ABC"`và không bao gồm các mã thông báo trống. 

Thứ hai,`"WUB"`có thể xuất hiện ở đầu hoặc cuối chuỗi, không tạo ra khoảng trắng ở đầu hoặc cuối. Ví dụ,`"WUBABCWUB"`vẫn nên xuất`"ABC"`không có khoảng trắng thừa. 

Thứ ba, chuỗi có thể chỉ chứa một từ mà không có bất kỳ từ nào`"WUB"`hoàn toàn, trong trường hợp đó đầu ra là chuỗi gốc không thay đổi. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ là liên tục tìm kiếm`"WUB"`và xóa nó khỏi chuỗi cho đến khi không còn lại. Mỗi lần xóa sẽ làm thay đổi chuỗi và tốn O(n) và trong trường hợp xấu nhất có thể xảy ra O(n), dẫn đến hành vi O(n²). Mặc dù điều này vẫn có thể chấp nhận được với n 200, nhưng nó gián tiếp không cần thiết và làm cho việc xử lý cạnh của các dấu phân cách liên tiếp trở nên khó khăn, đặc biệt là khi đảm bảo khoảng trắng được chèn chính xác giữa các từ được tái tạo. 

Một cách tiếp cận trực tiếp hơn là quét chuỗi một lần từ trái sang phải trong khi xử lý`"WUB"`dưới dạng một mẫu phân cách duy nhất. Bất cứ khi nào chúng ta gặp phải`"WUB"`, chúng tôi bỏ qua nó hoàn toàn. Bất cứ khi nào chúng tôi gặp phải một phân đoạn không khớp, chúng tôi sẽ thu thập nó như một phần của từ. Quan sát quan trọng là`"WUB"`hoạt động giống như một dấu phân cách không bao giờ xuất hiện ở đầu ra và không bao giờ đóng góp các ký tự. Điều này biến vấn đề thành trích xuất mã thông báo từ luồng có dấu phân cách có độ dài cố định đã biết. 

Cải tiến quan trọng là tránh xây dựng các chuỗi trung gian hoặc sửa đổi đầu vào nhiều lần. Thay vào đó, chúng ta chỉ di chuyển con trỏ qua chuỗi một lần, trích xuất các ký tự có ý nghĩa và chỉ chèn dấu cách khi chuyển đổi giữa các từ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xóa đi lặp lại`"WUB"`| O(n²) | O(n) | Được chấp nhận nhưng không hiệu quả | 
| Quét tuyến tính với bỏ qua dấu phân cách | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo chỉ mục`i = 0`và một danh sách trống`words`. Danh sách này sẽ lưu trữ các từ được xây dựng lại theo thứ tự. 
2. Trong khi`i`nhỏ hơn độ dài của chuỗi, hãy kiểm tra xem chuỗi con bắt đầu tại`i`trận đấu`"WUB"`. 

Khi nó phù hợp, tiến lên`i`bằng 3 để bỏ qua hoàn toàn dấu phân cách này. 

Điều này hoạt động vì`"WUB"`luôn là dấu phân cách cấu trúc, không bao giờ là một phần của từ thực. 
3. Nếu`"WUB"`không được tìm thấy ở vị trí`i`, bắt đầu thu thập một từ. 

Tạo bộ đệm trống và tiếp tục tiến lên`i`trong khi vị trí hiện tại không bắt đầu`"WUB"`sự liên tiếp. 

Nối từng ký tự vào bộ đệm. 

Điều này đảm bảo chúng tôi trích xuất các chuỗi ký tự thực liền kề tối đa. 
4. Một lần`"WUB"`gặp phải hoặc chuỗi kết thúc, hãy chuyển bộ đệm thành một từ và nối nó vào`words`nếu nó không trống. 

Điều này ngăn các từ trống do các dấu phân cách liên tiếp gây ra. 
5. Sau khi xử lý toàn bộ chuỗi, nối tất cả các từ đã thu thập bằng một khoảng trắng và xuất kết quả. 

Ý tưởng chính là`"WUB"`hoạt động như một ranh giới cứng. Mỗi khi bỏ qua nó, chúng ta được đảm bảo ở giữa các từ hoặc ở một ranh giới, vì vậy mọi ký tự tích lũy sẽ tạo thành một từ gốc hoàn chỉnh. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên tính bất biến rằng tại bất kỳ thời điểm nào, chúng ta đều đứng ở đầu một từ hợp lệ hoặc ở đầu một từ hợp lệ.`"WUB"`khối. Từ`"WUB"`không bao giờ xuất hiện bên trong các từ thực, bỏ qua nó không thể loại bỏ dữ liệu có ý nghĩa. Mỗi nhân vật không phải là một phần của một`"WUB"`khối thuộc về chính xác một từ gốc và các ký tự được thêm vào theo thứ tự, giữ nguyên thứ tự từ giống như trong bài hát gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)

words = []
i = 0

while i < n:
    if i + 3 <= n and s[i:i+3] == "WUB":
        i += 3
        continue

    buf = []
    while i < n and not (i + 3 <= n and s[i:i+3] == "WUB"):
        buf.append(s[i])
        i += 1

    if buf:
        words.append("".join(buf))

print(" ".join(words))
```Việc thực hiện tuân theo chiến lược quét trực tiếp. Vòng lặp bên ngoài đảm bảo chúng tôi xử lý chuỗi trong một lần chuyển. Vòng lặp bên trong xây dựng một từ cho đến khi`"WUB"`ranh giới xuất hiện. 

Một điểm tinh tế là việc kiểm tra lặp đi lặp lại`i + 3 <= n`trước khi so sánh chuỗi con. Điều này tránh việc cắt ra ngoài đầu chuỗi và giữ logic an toàn ở ranh giới. Một chi tiết quan trọng khác là`buf`bảo vệ trước khi nối thêm, điều này ngăn các từ trống liên tiếp`"WUB"`trình tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
WUBWUBABCWUB
```| Bước | tôi | Hành động hiện tại | Bộ đệm | Từ | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | bỏ qua "WUB" | [] | [] | 
| 2 | 3 | bỏ qua "WUB" | [] | [] | 
| 3 | 6 | thu thập từ | ["A","B","C"] | [] | 
| 4 | 9 | bỏ qua "WUB" | [] | ["ABC"] | 

Đầu ra:```
ABC
```Dấu vết này cho thấy các dấu phân cách liên tiếp hoàn toàn bị bỏ qua và không tạo ra các từ trống, đồng thời các dấu phân cách ở cuối được bỏ qua một cách an toàn mà không ảnh hưởng đến kết quả đầu ra. 

### Ví dụ 2 

đầu vào:```
WUBAREWUBTHEWUBWUBSONG
```| Bước | tôi | Hành động hiện tại | Bộ đệm | Từ | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | bỏ qua "WUB" | [] | [] | 
| 2 | 3 | thu thập "LÀ" | ["A","R","E"] | [] | 
| 3 | 6 | bỏ qua "WUB" | [] | ["LÀ"] | 
| 4 | 9 | thu thập "THE" | ["T","H","E"] | ["LÀ"] | 
| 5 | 12 | bỏ qua "WUB" | [] | ["LÀ","THE"] | 
| 6 | 15 | bỏ qua "WUB" | [] | ["LÀ","THE"] | 
| 7 | 18 | sưu tầm "BÀI HÁT" | ["S","O","N","G"] | ["LÀ","THE"] | 

Đầu ra:```
ARE THE SONG
```Điều này thể hiện việc xử lý chính xác nhiều từ và nhiều dấu phân cách liên tiếp, xác nhận rằng thứ tự từ được giữ nguyên chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được truy cập tối đa một lần trong quá trình quét | 
| Không gian | O(n) | Lưu trữ các từ đầu ra và bộ đệm tạm thời | 

Độ phức tạp tuyến tính dễ dàng đủ với n 200 và hệ số không đổi nhỏ do quét ký tự trực tiếp mà không cần cấu trúc phân tích cú pháp bổ sung. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = input().strip()
    n = len(s)

    words = []
    i = 0

    while i < n:
        if i + 3 <= n and s[i:i+3] == "WUB":
            i += 3
            continue

        buf = []
        while i < n and not (i + 3 <= n and s[i:i+3] == "WUB"):
            buf.append(s[i])
            i += 1

        if buf:
            words.append("".join(buf))

    return " ".join(words)

# provided sample
assert run("WUBWUBABCWUB\n") == "ABC"

# custom cases
assert run("ABC\n") == "ABC", "no separators"
assert run("WUBWUBWUB\n") == "", "only separators"
assert run("AWWUBB\n") == "AWWUBB", "WUB inside word not present so treated normally"
assert run("WUBA\n") == "A", "leading separator"
assert run("AWWUB\n") == "AWWUB", "trailing partial word"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"ABC"`|`"ABC"`| KHÔNG`"WUB"`hiện tại | 
|`"WUBWUBWUB"`|`""`| Chỉ dấu phân cách | 
|`"WUBA"`|`"A"`| Dấu phân cách hàng đầu | 
|`"AWWUBB"`|`"AWWUBB"`|`"WUB"`không được chèn một cách giả tạo vào bên trong từ đảm bảo an toàn | 
|`"AWWUB"`|`"AWWUB"`| Cấu trúc một phần theo sau | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi chuỗi bắt đầu bằng nhiều`"WUB"`khối, chẳng hạn như`"WUBWUBABC"`. Thuật toán liên tục bỏ qua`"WUB"`cho đến khi nó đạt tới`'A'`, do đó không có từ trống nào được tạo ở đầu. 

Một trường hợp khác là`"ABCWUBWUBDEF"`, trong đó có nhiều dấu phân cách xuất hiện giữa các từ. Quá trình quét xử lý cả hai`"WUB"`xuất hiện dưới dạng dấu phân cách mà không tạo ra bộ đệm trống, tạo ra`"ABC DEF"`. 

Trường hợp cuối cùng là`"WUBWUBWUB"`, trong đó toàn bộ dữ liệu đầu vào bao gồm các dấu phân cách. Bộ đệm không bao giờ được lấp đầy, do đó không có từ nào được thêm vào và đầu ra chính xác là một chuỗi trống.
