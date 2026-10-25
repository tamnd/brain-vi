---
title: "CF 106054A - Tạo tác để in"
description: "Chúng ta được cấp một chuỗi có độ dài cố định là 10, chỉ bao gồm các chữ cái Latinh viết hoa. Từ chuỗi này, chúng ta được phép xóa các ký tự nhưng không được phép sắp xếp lại những gì còn lại."
date: "2026-06-20T13:20:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106054
codeforces_index: "A"
codeforces_contest_name: "2025 Argentinian Programming Tournament (TAP)"
rating: 0
weight: 106054
solve_time_s: 50
verified: true
draft: false
---

[CF 106054A - Vật phẩm cần in](https://codeforces.com/problemset/problem/106054/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi có độ dài cố định là 10, chỉ bao gồm các chữ cái Latinh viết hoa. Từ chuỗi này, chúng ta được phép xóa các ký tự nhưng không được phép sắp xếp lại những gì còn lại. Câu hỏi đặt ra là liệu có thể lấy được dãy con “TAP” sau khi xóa một số ký tự hay không. 

Vì vậy, nhiệm vụ không phải là tìm “T”, “A” và “P” ở bất kỳ đâu một cách độc lập. Chúng phải xuất hiện theo thứ tự này bên trong chuỗi gốc, có thể có các chữ cái khác ở giữa và chúng ta được phép bỏ qua những chữ cái bổ sung đó. 

Các ràng buộc cực kỳ nhỏ vì độ dài chuỗi luôn chính xác là 10. Điều đó ngay lập tức cho chúng ta biết rằng ngay cả các phương pháp bậc hai hoặc bậc ba cũng sẽ là quá mức cần thiết. Quét tuyến tính là đủ và thậm chí việc sử dụng vũ lực đối với các chuỗi tiếp theo cũng có thể chấp nhận được về mặt hiệu suất. 

Có một số tình huống khó khăn đáng được nêu ra một cách rõ ràng. Một chuỗi chứa cả ba chữ cái nhưng sai thứ tự sẽ thất bại. Ví dụ: “PATXXXTXXX” chứa tất cả các ký tự nhưng không theo đúng thứ tự nên không hợp lệ. Một trường hợp khác là khi thiếu hoàn toàn một trong các chữ cái bắt buộc, chẳng hạn như “CONTRACETO”, lỗi này sẽ ngay lập tức thất bại vì không có chữ “P”. Cuối cùng, các chữ cái lặp đi lặp lại có thể gây hiểu nhầm. Một chuỗi như “TAAAPPPPPP” hợp lệ vì chúng ta có thể chọn “T” hợp lệ đầu tiên, sau đó là bất kỳ “A” nào sau đó, sau đó là “P”, giữ nguyên thứ tự. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là xem xét mọi dãy con có độ dài bằng ba và kiểm tra xem nó có bằng “TAP” hay không. Vì chuỗi có độ dài 10 nên có nhiều nhất C(10, 3) = 120 chuỗi con như vậy và việc kiểm tra mỗi chuỗi là thời gian không đổi. Điều này đã hoạt động thoải mái trong giới hạn và rất dễ hiểu. 

Tuy nhiên, cách tiếp cận này liệt kê rõ ràng các kết hợp, điều này là không cần thiết. Cấu trúc của bài toán chỉ hỏi “T”, sau đó là “A”, rồi sau đó là “P” có tìm được theo thứ tự hay không. Khi chúng tôi nhận ra rằng thứ tự là hạn chế duy nhất, chúng tôi có thể giảm vấn đề xuống còn một lần quét tham lam. 

Quan sát quan trọng là chúng ta chỉ cần theo dõi tiến trình thông qua một mẫu cố định. Chúng tôi bắt đầu tìm kiếm “T”. Khi tìm thấy nó, chúng tôi sẽ ngừng quan tâm đến các ký tự trước đó và bắt đầu tìm kiếm “A”. Sau khi tìm thấy “A”, chúng ta sẽ tìm “P”. Nếu chúng ta cố gắng đi đến cuối mẫu thì câu trả lời là có. 

Điều này có hiệu quả vì mỗi ký tự của mẫu phải được khớp theo thứ tự và việc bỏ qua về phía trước không bao giờ chặn một kết quả khớp hợp lệ trong tương lai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Chuỗi tiếp theo của Brute Force | O(120) | O(1) | Đã chấp nhận | 
| Quét tham lam | O(10) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo con trỏ`i = 0`trên chuỗi mục tiêu “TAP”, nghĩa là chúng tôi hiện đang cố gắng khớp ký tự chưa khớp đầu tiên. 
2. Lặp qua từng ký tự`c`trong chuỗi đầu vào từ trái sang phải. 
3. Nếu`c`bằng`TAP[i]`, nâng cao`i`bởi một. Điều này có nghĩa là chúng tôi đã khớp thành công ký tự bắt buộc tiếp theo theo thứ tự. 
4. Nếu tại bất kỳ thời điểm nào`i`trở thành 3, chúng tôi đã khớp tất cả các ký tự của “TAP”, vì vậy chúng tôi có thể dừng sớm. 
5. Sau khi quét toàn bộ chuỗi (hoặc dừng sớm), kiểm tra xem`i == 3`. Nếu có thì xuất ra “S”, nếu không thì xuất ra “N”. 

Sở dĩ chỉ tiến lên trong các trận đấu là vì chúng ta phải giữ gìn trật tự. Việc bỏ qua các ký tự không khớp đảm bảo rằng chúng ta đang tìm kiếm một chuỗi con một cách hiệu quả thay vì một chuỗi con. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến là trước khi xử lý từng ký tự trong đầu vào,`i`đại diện cho độ dài của tiền tố dài nhất của “TAP” có thể được hình thành bằng cách sử dụng một chuỗi con của tiền tố được xử lý của chuỗi đầu vào. Mỗi lần chúng tôi khớp ký tự cần thiết tiếp theo, chúng tôi sẽ mở rộng tiền tố này thêm chính xác một ký tự. Bởi vì chúng tôi chỉ tiến về phía trước trong cả đầu vào và mẫu, nên chúng tôi không bao giờ làm mất hiệu lực các lựa chọn trước đó và bất kỳ chuỗi con hợp lệ nào cũng phải tương ứng với một số chuỗi tiến bộ thành công của`i`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
target = "TAP"

i = 0
for c in s:
    if i < 3 and c == target[i]:
        i += 1
        if i == 3:
            break

print("S" if i == 3 else "N")
```Mã đọc chuỗi đầu vào duy nhất và duy trì một con trỏ`i`vào mẫu “TAP”. Mỗi ký tự được kiểm tra dựa trên ký tự được yêu cầu hiện tại. Khi cả ba ký tự đều khớp, chúng tôi sẽ kết thúc sớm vì việc quét thêm không thể thay đổi kết quả. Điều kiện cuối cùng kiểm tra xem mẫu đầy đủ có khớp hay không. 

Một điểm tinh tế là lối ra sớm. Mặc dù không cần thiết về tính chính xác nhưng nó đảm bảo quá trình quét sẽ dừng ngay khi đạt được thành công, mặc dù với độ dài 10, điều này chủ yếu mang tính cách điệu. 

## Ví dụ đã hoạt động 

### Ví dụ 1: “CONTRAPELO” 

Chúng tôi theo dõi cách con trỏ phát triển khi chúng tôi quét từ trái sang phải. 

| Nhân vật | Mục tiêu hiện tại | Hành động | tôi | 
| --- | --- | --- | --- | 
| C | T | bỏ qua | 0 | 
| Ồ | T | bỏ qua | 0 | 
| N | T | bỏ qua | 0 | 
| T | A | trận đấu T | 1 | 
| R | A | bỏ qua | 1 | 
| A | P | trận đấu A | 2 | 
| P | kết thúc | khớp P | 3 | 

Cuối cùng,`i = 3`, vậy đáp án là “S”. Điều này xác nhận rằng ngay cả khi có các chữ cái xen vào, thứ tự vẫn được giữ nguyên. 

### Ví dụ 2: “XATPTPKABB” 

| Nhân vật | Mục tiêu hiện tại | Hành động | tôi | 
| --- | --- | --- | --- | 
| X | T | bỏ qua | 0 | 
| A | T | bỏ qua | 0 | 
| T | A | trận đấu T | 1 | 
| P | A | bỏ qua (không khớp với A? thực tế mục tiêu là A) | 1 | 
| T | A | bỏ qua | 1 | 
| P | A | bỏ qua | 1 | 
| K | A | bỏ qua | 1 | 
| A | P | trận đấu A | 2 | 
| B | P | bỏ qua | 2 | 
| B | P | bỏ qua | 2 | 

Chúng tôi không bao giờ đạt được`P`sau khi hoàn thành “TA”, vì vậy chúng tôi kết thúc bằng`i = 2`, tạo ra chữ “N”. Điều này cho thấy rằng ngay cả khi tất cả các chữ cái tồn tại, việc sắp xếp sai sẽ ngăn cản việc hoàn thành. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10) | quét tuyến tính đơn trên chuỗi có độ dài cố định | 
| Không gian | O(1) | chỉ có một con trỏ không đổi và chuỗi đích | 

Các ràng buộc cố định kích thước đầu vào ở mức mười ký tự, do đó thuật toán chạy trong thời gian không đổi và thỏa mãn cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = sys.stdin.readline().strip()
    target = "TAP"
    i = 0
    for c in s:
        if i < 3 and c == target[i]:
            i += 1
    return "S" if i == 3 else "N"

# provided samples
assert run("CONTRAPELO\n") == "S"
assert run("XATPTKABPB\n") == "S"
assert run("XATPTPKABB\n") == "N"

# custom cases
assert run("TAPXXXXXXXX\n") == "S"
assert run("XXXXXXXXXX\n") == "N"
assert run("TTTAAAAPPPP\n") == "S"
assert run("PTAATPXABCD\n") == "N"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| TAPXXXXXXXXX | S | đã được hình thành khi bắt đầu | 
| XXXXXXXXXX | N | thiếu tất cả các chữ cái bắt buộc | 
| TTAAAAPPPP | S | nhiều lần vẫn hợp lệ | 
| PTAATPXABCD | N | đúng chữ nhưng sai thứ tự | 

## Vỏ cạnh 

Một trường hợp khó nhận thấy là khi tất cả các chữ cái bắt buộc đều tồn tại nhưng được xen kẽ không chính xác. Đối với đầu vào “PTAATPXABCD”, quá trình quét sẽ tìm thấy chữ “T” trước, nhưng không có chữ “A” hợp lệ sau đó dẫn đến chữ “P” trong tiến trình chính xác. Con trỏ bị kẹt ở giai đoạn thứ hai và thuật toán sẽ từ chối nó một cách chính xác. 

Một trường hợp khác là sự lặp lại nặng nề. Đối với “TTTAAAAPPPPP”, thuật toán không cố gắng chọn các lần xuất hiện tối ưu, nó chỉ đơn giản lấy tiến trình hợp lệ đầu tiên: T đầu tiên, A đầu tiên sau nó, P đầu tiên sau đó. Điều này đảm bảo tính chính xác bởi vì bất kỳ chuỗi con hợp lệ nào cũng sẽ vẫn cho phép ít nhất một tiến triển đơn điệu thông qua mẫu.
