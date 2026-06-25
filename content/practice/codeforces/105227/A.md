---
title: "CF 105227A - LLPS"
description: "Chúng ta được cấp một chuỗi đơn gồm các chữ cái viết thường. Từ chuỗi này, chúng ta có thể xóa các ký tự trong khi vẫn giữ nguyên thứ tự, tạo ra bất kỳ chuỗi con nào."
date: "2026-06-24T16:30:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105227
codeforces_index: "A"
codeforces_contest_name: "CPG Training Contest - 1"
rating: 0
weight: 105227
solve_time_s: 315
verified: false
draft: false
---

[CF 105227A - LLPS](https://codeforces.com/problemset/problem/105227/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 15 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi đơn gồm các chữ cái viết thường. Từ chuỗi này, chúng ta có thể xóa các ký tự trong khi vẫn giữ nguyên thứ tự, tạo ra bất kỳ chuỗi con nào. Trong số tất cả các dãy con như vậy, chúng ta muốn một dãy con có tính chất palindrome và, trong số tất cả các dãy con palindrome, chúng ta muốn dãy con lớn nhất về mặt từ điển. 

Độ dài chuỗi tối đa là 10, do đó đầu vào rất nhỏ. Điều đó ngay lập tức thay đổi bản chất của vấn đề: việc khám phá theo cấp số nhân đã khả thi, nhưng chúng ta vẫn nên hướng tới một cái nhìn sâu sắc về cấu trúc rõ ràng thay vì ép buộc tất cả các chuỗi con. 

Thứ tự từ điển ở đây tuân theo quy tắc chuẩn: so sánh từ trái sang phải và vị trí đầu tiên nơi các ký tự khác nhau sẽ quyết định hoặc nếu ký tự này là tiền tố của ký tự kia thì chuỗi dài hơn sẽ lớn hơn. 

Một cạm bẫy ngây thơ là cho rằng chúng ta cần “xây dựng” một palindrome một cách tham lam từ cả hai đầu. Điều đó không thành công vì các lựa chọn tối ưu phụ thuộc vào cấu trúc tổng thể chứ không phải sự phù hợp cục bộ. Một sai lầm khác là suy nghĩ lâu hơn luôn thắng; điều đó là sai vì một chuỗi dài hơn có thể bắt đầu bằng một ký tự nhỏ hơn, làm mất đi từ điển ngay lập tức. 

Trường hợp cạnh nhỏ xuất hiện khi tất cả các ký tự đều khác biệt. Khi đó, mọi dãy con palindrome đều có độ dài 1 và câu trả lời chỉ là ký tự tối đa. Một trường hợp khác là khi câu trả lời tốt nhất không phải là bảng màu dài nhất, như đã thấy trong mẫu “codeforces” trong đó câu trả lời là “s”. 

## Phương pháp tiếp cận 

Chế độ xem brute-force rất đơn giản: liệt kê tất cả các chuỗi con của chuỗi, kiểm tra xem mỗi chuỗi có phải là một bảng màu hay không và giữ thứ tự tốt nhất theo thứ tự từ điển. Có nhiều nhất 2ⁿ dãy con. Đối với mỗi loại, việc kiểm tra palindrome mất O(n), do đó độ phức tạp tổng cộng là O(n·2ⁿ). Với n 10, tối đa là khoảng 10.240 lần kiểm tra, điều này hoàn toàn ổn trong thực tế. 

Tuy nhiên, cách tiếp cận bạo lực này về mặt khái niệm là quá mức cần thiết. Cấu trúc của các chuỗi con đối xứng trong một chuỗi có một sự đơn giản hóa quan trọng: bất kỳ chuỗi đối xứng nào đều có các ký tự đầu tiên và cuối cùng trùng khớp, đồng thời các chuỗi lớn hơn về mặt từ điển sẽ ưu tiên ký tự đầu tiên hơn mọi ký tự khác. Vì vậy trước tiên chúng ta nên hiểu ký tự đầu tiên của câu trả lời phải là gì. 

Nếu chúng ta chọn bất kỳ ký tự c nào làm ký tự đầu tiên của bảng màu thì ký tự cuối cùng cũng phải là c và cả hai đều phải xuất phát từ sự xuất hiện của c trong chuỗi. Do đó, bảng màu lớn nhất về mặt từ điển phải bắt đầu bằng ký tự lớn nhất có thể xuất hiện ít nhất một lần. Khi chúng ta chọn ký tự c như vậy, chúng ta luôn có thể tạo thành một dãy con palindrome hợp lệ chỉ bao gồm một lần xuất hiện của c hoặc hai lần xuất hiện của c nếu có ít nhất hai lần xuất hiện. 

Bây giờ hãy so sánh các khả năng. Nếu c xuất hiện ít nhất hai lần, chúng ta có thể tạo thành “cc”. Bất kỳ bảng màu nào dài hơn bắt đầu bằng c cũng phải bắt đầu bằng c và sau đó tiếp tục với thứ gì đó giữa hai lần xuất hiện đã chọn. Nhưng bất kỳ phần bên trong nào cũng không được giới thiệu một ký tự nhỏ hơn c ở phía trước, nếu không thì thứ tự từ điển sẽ không được cải thiện. Vì c đã là ký tự tối đa trong chuỗi nên không gì có thể đánh bại một chuỗi bắt đầu bằng c và giữ lại nhiều nhất có thể chuỗi c, chuỗi này thu gọn thành lặp lại c. 

Do đó, bài toán rút gọn thành việc tìm ký tự lớn nhất trong chuỗi và đếm xem nó xuất hiện bao nhiêu lần. Nếu nó xuất hiện một lần thì đáp án chính là ký tự đó. Nếu nó xuất hiện k lần thì palindrome tốt nhất là ký tự đó được lặp lại k lần, vì chúng ta có thể chọn tất cả các lần xuất hiện dưới dạng một dãy con và về cơ bản nó là một palindrome. 

Đây là sự sụp đổ cấu trúc quan trọng: thay vì khám phá các palindrome, chúng ta chỉ cần tần số của ký tự tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·2ⁿ) | O(n) | Được chấp nhận nhưng không cần thiết | 
| Tần số char tối đa | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xử lý chuỗi một lần để xác định ký tự lớn nhất hiện diện và số lần nó xuất hiện. 

1. Quét chuỗi và duy trì ký tự tối đa được thấy cho đến nay. Điều này đúng vì tính tối ưu của từ điển trước hết phụ thuộc vào ký tự dẫn đầu, vì vậy chúng ta phải tối đa hóa nó trên toàn cục. 
2. Đếm xem ký tự tối đa này xuất hiện bao nhiêu lần. Điều này quan trọng vì việc lặp lại cùng một ký tự tối đa sẽ bảo toàn cả cấu trúc palindrome và giá trị từ điển. 
3. Xây dựng một chuỗi bao gồm ký tự này được lặp lại nhiều lần khi nó xuất hiện trong đầu vào. 
4. Xuất trực tiếp chuỗi đã xây dựng này vì đây là một dãy con hợp lệ và một bảng màu. 

Phần không rõ ràng là lý do tại sao chúng ta được phép lấy tất cả các lần xuất hiện của ký tự tối đa. Bất kỳ dãy con nào chỉ gồm các ký tự giống nhau luôn luôn là một dãy palindrome, vì việc đảo ngược nó không thay đổi gì cả. Việc xóa bất kỳ sự xuất hiện nào sẽ chỉ rút ngắn chuỗi mà không cải thiện thứ tự từ điển vì tất cả các ký tự đều bằng nhau. Vì vậy, mức tối đa giảm xuống mức độ dài tối đa giữa các ứng cử viên có khởi đầu bằng nhau. 

### Tại sao nó hoạt động 

Bất kỳ dãy con đối xứng nào cũng có ký tự đầu tiên và thứ tự từ điển được quyết định ngay lập tức bởi ký tự đó trừ khi cả hai ứng cử viên đều có chung ký tự đó. Do đó, giải pháp tối ưu phải bắt đầu bằng ký tự tối đa có thể có trong chuỗi. Sau khi bị giới hạn ở ký tự đó, không ký tự nào khác có thể cải thiện thứ tự từ điển và chỉ thêm các ký tự giống hệt nhau sẽ duy trì cả tính hợp lệ và tính tối ưu. Điều này buộc câu trả lời phải là tập hợp nhiều lần xuất hiện của ký tự tối đa được sắp xếp dưới dạng một bảng màu, đơn giản là sự lặp lại của ký tự đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

mx = max(s)
cnt = s.count(mx)

print(mx * cnt)
```Trước tiên, giải pháp tính toán ký tự tối đa bằng cách sử dụng tính năng so sánh tích hợp sẵn của Python đối với các ký tự, hoạt động trực tiếp vì thứ tự ASCII khớp với thứ tự từ điển. Sau đó nó đếm số lần xuất hiện của ký tự đó. Đầu ra được xây dựng bằng cách lặp lại ký tự. 

Một điểm tinh tế là chúng ta không cần xác minh rõ ràng tính chất palindrome, bởi vì bất kỳ chuỗi nào bao gồm một ký tự lặp lại luôn luôn là một palindrome. Một điều tinh tế khác là chúng ta không cần phải xem xét các ràng buộc về dãy con một cách rõ ràng: mọi lần xuất hiện của ký tự lớn nhất đều có thể được chọn theo thứ tự, do đó sự lặp lại luôn có thể đạt được dưới dạng dãy con. 

## Ví dụ đã hoạt động 

Hãy xem xét "ra-đa". 

Chúng tôi quét và tìm ký tự tối đa là ‘r’. Nó xuất hiện hai lần. 

| bước | char tối đa | đếm | 
| --- | --- | --- | 
| r | r | 1 | 
| một | r | 1 | 
| d | r | 1 | 
| một | r | 1 | 
| r | r | 2 | 

Kết quả là “rr”. Điều này cho thấy rằng mặc dù bản thân “ra-đa” là một bảng màu, nhưng một dãy con lớn hơn về mặt từ điển có thể được hình thành bằng cách chỉ tập trung vào ký tự nổi bật. 

Bây giờ hãy xem xét “lực lượng mã hóa”. 

Ký tự tối đa là ‘s’, xuất hiện một lần. 

| bước | char tối đa | đếm | 
| --- | --- | --- | 
| c | c | 1 | 
| o | o | 1 | 
| d | o | 1 | 
| e | o | 1 | 
| f | o | 1 | 
| o | o | 1 | 
| r | r | 1 | 
| c | r | 1 | 
| e | r | 1 | 
| s | s | 1 | 

Đầu ra là “s”, vì không có ký tự lặp lại nào tồn tại để tạo thành một bảng màu dài hơn. 

Những dấu vết này xác nhận rằng thuật toán hoàn toàn được điều khiển bởi sự thống trị của ký tự tối đa và bỏ qua tất cả các cấu trúc khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | lần vượt qua cộng với số lần đếm | 
| Không gian | O(1) | chỉ có bộ đếm cố định và ký tự tối đa | 

Kích thước đầu vào tối đa là 10, do đó, ngay cả các giải pháp tầm thường cũng dễ dàng vượt qua, nhưng cách tiếp cận tuyến tính này tổng quát hóa rõ ràng và tránh mọi phép liệt kê. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    s = input().strip()
    mx = max(s)
    cnt = s.count(mx)
    return mx * cnt

# provided samples
assert run("radar\n") == "rr"
assert run("bowwowwow\n") == "wwwww"
assert run("codeforces\n") == "s"

# custom cases
assert run("a\n") == "a", "single character"
assert run("abcabc\n") == "cc", "multiple max letters"
assert run("zzxyzz\n") == "zzzz", "all max occurrences used"
assert run("abcd\n") == "d", "strictly increasing letters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một | một | chuỗi có độ dài tối thiểu | 
| abcabc | cc | lựa chọn ký tự tối đa lặp đi lặp lại | 
| zzxyzz | zzzz | nhiều lần xuất hiện tối đa chiếm ưu thế | 
| abcd | d | tối đa duy nhất ở cuối | 

## Vỏ cạnh 

Đối với chuỗi ký tự đơn như “a”, thuật toán xác định ‘a’ vừa là ký tự tối đa vừa là ký tự duy nhất, tạo ra “a”. Cấu trúc vẫn hoạt động vì số lần lặp lại là một nên không cần xử lý đặc biệt. 

Đối với các chuỗi có ký tự tối đa xuất hiện nhiều lần, chẳng hạn như “zzxyzz”, quá trình quét sẽ tích lũy chính xác tất cả các lần xuất hiện của ‘z’. Vì tất cả đều bằng nhau nên thứ tự giữa chúng không quan trọng và chuỗi lặp lại thu được luôn là một chuỗi palindrome và tối đa về mặt từ điển trong số các chuỗi con hợp lệ.
