---
title: "CF 103687B - JB Yêu Dấu Phẩy"
description: "Chúng ta được cung cấp một chuỗi duy nhất chỉ bao gồm các chữ cái tiếng Anh viết thường. Nhiệm vụ là quét chuỗi này từ trái sang phải và bất cứ khi nào các ký tự liên tiếp tạo thành chuỗi con \"cjb\" thì chúng ta phải chèn dấu phẩy ngay sau lần xuất hiện đó."
date: "2026-07-02T20:56:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103687
codeforces_index: "B"
codeforces_contest_name: "The 19th Zhejiang Provincial Collegiate Programming Contest"
rating: 0
weight: 103687
solve_time_s: 43
verified: true
draft: false
---

[CF 103687B - JB Yêu Dấu phẩy](https://codeforces.com/problemset/problem/103687/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi duy nhất chỉ bao gồm các chữ cái tiếng Anh viết thường. Nhiệm vụ là quét chuỗi này từ trái sang phải và bất cứ khi nào các ký tự liên tiếp tạo thành chuỗi con`"cjb"`, chúng ta phải chèn dấu phẩy ngay sau lần xuất hiện đó. Phần còn lại của chuỗi không thay đổi và các lần xuất hiện chồng chéo hoặc lặp lại phải được xử lý theo cách tôn trọng cấu trúc từ trái sang phải của đầu ra. 

Kích thước đầu vào có thể lớn tới 100000 ký tự, điều đó có nghĩa là bất kỳ giải pháp nào liên tục xây dựng chuỗi con hoặc thực hiện quét lồng nhau qua chuỗi sẽ quá chậm. Cần phải có một thuật toán tuyến tính theo độ dài của chuỗi, vì khoảng 100000 thao tác là mức ngân sách thoải mái duy nhất trong giới hạn thời gian 1 giây trong Python. 

Một cách tiếp cận đơn giản là kiểm tra mọi vị trí và xây dựng lại chuỗi bằng cách cắt có thể thất bại theo những cách tinh tế. Một vấn đề là hiệu suất bị suy giảm do nối chuỗi lặp đi lặp lại, trong trường hợp xấu nhất sẽ trở thành bậc hai. Một vấn đề khác là việc xử lý không chính xác các mẫu chồng chéo nếu một người cố gắng sửa đổi chuỗi tại chỗ trong khi lặp lại. 

Ví dụ, hãy xem xét đầu vào`"cjbjb"`. Chuỗi con`"cjb"`xuất hiện lúc đầu. Sau khi chèn dấu phẩy, chúng ta nhận được`"cjb,jb"`. Cách tiếp cận bất cẩn làm thay đổi chỉ số không chính xác có thể bỏ qua các ký tự hoặc cố gắng khớp lại bên trong vùng đã sửa đổi, tạo ra dấu phẩy trùng lặp hoặc thiếu kết quả khớp hợp lệ. 

Một trường hợp cạnh khác là một chuỗi không có lần xuất hiện nào, chẳng hạn như`"abcdef"`, trong đó đầu ra phải giống với đầu vào. Cách tiếp cận không chính xác vẫn có thể tạo ra các dấu phân cách do việc kiểm tra ranh giới bị lỗi. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: lặp lại mọi chỉ mục`i`trong chuỗi và với mỗi vị trí, hãy kiểm tra xem chuỗi con bắt đầu tại`i`trận đấu`"cjb"`. Nếu có, hãy nối thêm`"cjb,"`đến kết quả và bỏ qua phía trước một cách thích hợp. Nếu không, hãy nối thêm ký tự hiện tại. 

Cách tiếp cận này đúng vì nó tuân theo định nghĩa của nhiệm vụ. Vấn đề là hiệu quả trong cách xây dựng kết quả và cách nâng cao chỉ số. Nếu được triển khai bằng cách nối chuỗi lặp lại, mỗi thao tác nối thêm có thể tốn O(n), khiến trường hợp xấu nhất là O(n²). Đối với n lên tới 100000, điều này quá chậm. 

Quan sát quan trọng là chúng ta không bao giờ cần phải xem lại các ký tự trước đó và chúng ta không bao giờ cần duy trì bất kỳ trạng thái phức tạp nào ngoài một số ký tự cuối cùng được nhìn thấy. Điều này cho phép chúng ta xử lý chuỗi trong một lần truyền trong khi vẫn duy trì một bộ đệm cuộn nhỏ gồm các ký tự gần đây. Khi bộ đệm khớp`"cjb"`, chúng ta phát ra nó và chèn dấu phẩy vào ngay. 

Điều này biến vấn đề thành một nhiệm vụ xây dựng luồng: chúng tôi xây dựng đầu ra tăng dần, chỉ kiểm tra ba ký tự cuối cùng ở mỗi bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng ký tự chuỗi trong khi duy trì bộ đệm đầu ra ngày càng tăng. 

1. Khởi tạo danh sách trống`out`để lưu trữ các ký tự đầu ra một cách hiệu quả. Danh sách được sử dụng thay vì chuỗi để tránh chi phí nối bậc hai. 
2. Lặp qua từng ký tự`ch`trong chuỗi đầu vào, nối nó vào`out`. Thêm vào là O(1). 
3. Sau mỗi lần nối thêm, hãy kiểm tra xem ba ký tự cuối cùng trong`out`hình thức`"cjb"`. Kiểm tra này chỉ hợp lệ khi độ dài hiện tại ít nhất là 3. 
4. Nếu tìm thấy kết quả khớp, hãy thêm dấu phẩy vào`out`. Chúng tôi không xóa các ký tự hoặc quay lại vì mẫu có độ dài cố định và chúng tôi muốn giữ nguyên các ký tự gốc cộng với dấu phẩy được chèn vào. 
5. Tiếp tục cho đến hết chuỗi. 
6. Tham gia danh sách`out`thành một chuỗi cuối cùng và xuất nó. 

Lý do bước 3 an toàn là vì chúng tôi chỉ cần phát hiện các mẫu kết thúc ở vị trí hiện tại. Vì chúng tôi xử lý từ trái sang phải và không bao giờ xem lại các phần trước đó của đầu ra nên mọi lần xuất hiện hợp lệ đều phải kết thúc ở chỉ mục hiện tại. 

### Tại sao nó hoạt động 

Ở mỗi bước, tiền tố được xây dựng của`out`chính xác là phiên bản được chuyển đổi của tiền tố tương ứng của chuỗi đầu vào theo quy tắc “chèn dấu phẩy sau mỗi lần xuất hiện của chuỗi đầu vào”.`cjb`đã thấy cho đến nay”. Thuật toán không bao giờ xóa hoặc sắp xếp lại các ký tự mà chỉ thêm các ký tự mới. Bất kỳ sự xuất hiện nào của`"cjb"`trong đầu vào cũng xuất hiện liền kề trong đầu ra và việc kiểm tra đảm bảo rằng dấu phẩy được chèn chính xác một lần cho mỗi lần xuất hiện, tại thời điểm đầu tiên, dấu phẩy sẽ hiển thị ở cuối luồng. Điều này ngăn cản cả việc thiếu kết quả trùng khớp và chèn trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

out = []

for ch in s:
    out.append(ch)
    if len(out) >= 3 and out[-3] == 'c' and out[-2] == 'j' and out[-1] == 'b':
        out.append(',')

print(''.join(out))
```Giải pháp dựa trên việc xây dựng tăng dần bằng cách sử dụng danh sách, giúp tránh việc sao chép chuỗi lặp lại. Việc kiểm tra mẫu chỉ được thực hiện trên ba ký tự cuối cùng, khiến cho thời gian lặp lại không đổi. 

Chi tiết triển khai tinh tế là việc sử dụng danh sách thay vì bộ tích lũy chuỗi. sử dụng`ans += ch`nhiều lần sẽ làm giảm hiệu suất đáng kể do phân bổ lặp đi lặp lại. Một chi tiết quan trọng khác là kiểm tra độ dài trước khi lập chỉ mục`out[-3]`, giúp tránh lỗi thời gian chạy đối với tiền tố ngắn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`pbpbppb`| tôi | char | out sau khi nối thêm | cuộc thi đấu`"cjb"`| ra sau bước | 
| --- | --- | --- | --- | --- | 
| 0 | p | p | không | p | 
| 1 | b | pb | không | pb | 
| 2 | p | pbp | không | pbp | 
| 3 | b | pbpb | không | pbpb | 
| 4 | p | pbpbp | không | pbpbp | 
| 5 | p | pbpbpp | không | pbpbpp | 
| 6 | b | pbpbppb | không | pbpbppb | 

Ví dụ này chứng minh rằng khi không`"cjb"`xuất hiện, đầu ra không thay đổi. 

### Ví dụ 2 

đầu vào:`cjbismyson`| tôi | char | out sau khi nối thêm | cuộc thi đấu`"cjb"`| ra sau bước | 
| --- | --- | --- | --- | --- | 
| 0 | c | c | không | c | 
| 1 | j | cj | không | cj | 
| 2 | b | cjb | vâng | cjb, | 
| 3 | tôi | cjb, tôi | không | cjb, tôi | 
| 4 | s | cjb,là | không | cjb,là | 
| 5 | m | cjb, chủ nghĩa | không | cjb, chủ nghĩa | 
| 6 | y | cjb,ismy | không | cjb,ismy | 
| 7 | s | cjb,ismys | không | cjb,ismys | 
| 8 | o | cjb,ismyso | không | cjb,ismyso | 
| 9 | n | cjb,ismyson | không | cjb,ismyson | 

Dấu vết này cho thấy dấu phẩy được chèn ngay sau khi phát hiện`"cjb"`và không ảnh hưởng đến quá trình xử lý tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần và mỗi bước bao gồm việc kiểm tra và nối thêm theo thời gian liên tục | 
| Không gian | O(n) | Bộ đệm đầu ra lưu trữ chuỗi đã chuyển đổi | 

Quá trình quét tuyến tính vừa vặn thoải mái trong các giới hạn cho n lên tới 100000 và mức sử dụng bộ nhớ vẫn tỷ lệ thuận với kích thước đầu ra. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = sys.stdin.readline().strip()
    out = []
    for ch in s:
        out.append(ch)
        if len(out) >= 3 and out[-3] == 'c' and out[-2] == 'j' and out[-1] == 'b':
            out.append(',')
    return ''.join(out)

# provided samples
assert run("pbpbppb\n") == "pbpbppb"
assert run("cjbismyson\n") == "cjb,ismyson"

# custom cases
assert run("cjb\n") == "cjb,"
assert run("ccjb\n") == "ccjb,"
assert run("cjcjb\n") == "cjcjb,"
assert run("abccjbb\n") == "abccjb,b"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`cjb`|`cjb,`| trận đấu duy nhất ở cuối | 
|`ccjb`|`ccjb,`| trận đấu không bắt đầu ở chỉ số 0 ca | 
|`cjcjb`|`cjcjb,`| hành vi an toàn chồng chéo | 
|`abccjbb`|`abccjb,b`| tương tác nhiều ký tự gần mẫu | 

## Vỏ cạnh 

Đối với đầu vào`"cjb"`, thuật toán nối thêm`c`,`j`,`b`một cách tuần tự. Ở ký tự thứ ba, bộ đệm kết thúc bằng`"cjb"`, do đó dấu phẩy được thêm vào ngay lập tức, tạo ra`"cjb,"`. Không có ký tự nào tồn tại nữa nên quá trình kết thúc rõ ràng. 

Đối với đầu vào`"ccjb"`, bộ đệm phát triển như`"c" → "cc" → "ccj" → "ccjb"`. Chỉ ở bước cuối cùng thì hậu tố mới khớp, tạo ra`"ccjb,"`. Điều này xác nhận rằng thuật toán không yêu cầu căn chỉnh mẫu ở các vị trí cố định mà chỉ khớp hậu tố. 

Đối với đầu vào`"cjcjb"`, thuật toán đảm bảo xử lý chính xác cấu trúc chồng chéo. Sau khi xử lý`"cjc"`, không có sự trùng khớp nào xảy ra. Khi`"cjb"`được hình thành ở cuối, chỉ lần xuất hiện cuối cùng mới kích hoạt dấu phẩy. Điều này cho thấy các tiền tố một phần trước đó không can thiệp vào các kết quả khớp hợp lệ sau này.
