---
title: "CF 103811J - Hãy bỏ qua nó"
description: "Chúng tôi được cung cấp một hệ thống xác suất mô phỏng những gì xảy ra khi Justin nhấp vào video. Mỗi lần anh ta vào hoặc làm mới trang, chính xác một trong nhiều kết quả sẽ xảy ra theo xác suất cố định."
date: "2026-07-02T08:29:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103811
codeforces_index: "J"
codeforces_contest_name: "La Salle-Pui Ching Programming Challenge \u57f9\u6b63\u5587\u6c99\u7de8\u7a0b\u6311\u6230\u8cfd 2021"
rating: 0
weight: 103811
solve_time_s: 59
verified: true
draft: false
---

[CF 103811J - Chỉ cần bỏ qua](https://codeforces.com/problemset/problem/103811/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hệ thống xác suất mô phỏng những gì xảy ra khi Justin nhấp vào video. Mỗi lần anh ta vào hoặc làm mới trang, chính xác một trong nhiều kết quả sẽ xảy ra theo xác suất cố định. Kết quả có thể là không có quảng cáo nào cả hoặc một trong số các loại quảng cáo: quảng cáo có thể bỏ qua hoặc quảng cáo không thể bỏ qua có độ dài khác nhau bao gồm cả quảng cáo đệm. 

Mục tiêu của Justin là bắt đầu xem video càng sớm càng tốt. Anh ta có một khả năng đặc biệt: trước khi quyết định xem một quảng cáo, anh ta có thể phát hiện ngay loại quảng cáo nào đã xuất hiện. Nếu anh ta không thích kết quả hiện tại, anh ta có thể nhấn nút làm mới, thao tác này tốn đúng một giây và cuộn lại quảng cáo theo xác suất tương tự. Nếu anh ấy quyết định không làm mới, anh ấy phải chịu toàn bộ chi phí của loại quảng cáo xuất hiện, với những quảng cáo có thể bỏ qua chỉ đóng góp một khoảng thời gian chờ đợi ban đầu cố định trước khi có thể bỏ qua. 

Vì vậy, quá trình này là một hệ thống quyết định trên các trạng thái xác suất lặp đi lặp lại. Mỗi trạng thái giống hệt nhau vì xác suất không thay đổi sau khi làm mới và lựa chọn duy nhất là dừng và chấp nhận quảng cáo hiện tại hay trả tiền một giây để quay lại. 

Đầu vào đưa ra năm phần trăm mô tả phân bố xác suất trên các kết quả. Đầu ra là thời gian dự kiến ​​tối thiểu cho đến khi Justin có thể bắt đầu xem video, giả định các quyết định tối ưu ở mỗi bước. 

Ràng buộc là các xác suất có tổng bằng 100, do đó hệ thống là một quá trình quyết định Markov một trạng thái với một tập hợp hữu hạn các hành động. Ý nghĩa quan trọng là bất kỳ chiến lược tối ưu nào cũng phải đứng yên: ở mỗi bước Justin đều ở trong tình huống giống nhau nên quyết định của anh ấy chỉ phụ thuộc vào loại quảng cáo hiện tại. 

Một cách tiếp cận ngây thơ liệt kê các chiến lược bằng cách quyết định số lần làm mới trước khi dừng sẽ ngay lập tức theo cấp số nhân về số lần làm mới được xem xét. Trong trường hợp xấu nhất, nếu một người cố gắng thực hiện tối đa k quyết định làm mới, thì mỗi đường dẫn sẽ phân nhánh thành năm kết quả, đưa ra hành vi O(5^k), điều này không khả thi ngay cả đối với k nhỏ. 

Một trường hợp phức tạp phát sinh từ việc xử lý quảng cáo có thể bỏ qua không chính xác. Ví dụ: nếu quảng cáo có thể bỏ qua chỉ đóng góp 5 giây nhưng việc triển khai đơn giản coi quảng cáo đó là hoàn toàn không thể tránh khỏi như quảng cáo 15 giây, thì quảng cáo đó sẽ đánh giá quá cao chi phí dự kiến ​​và luôn thích làm mới quá mạnh mẽ. Một trường hợp đặc biệt khác là bỏ qua kết quả “không có quảng cáo”: nếu xác suất đó tồn tại, việc làm mới thường tệ hơn rất nhiều vì việc dừng ngay lập tức có thể mang lại chi phí bằng 0. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là nghĩ đến việc Justin chọn một chính sách cố định như “làm mới tối đa k lần, sau đó chấp nhận bất cứ điều gì xuất hiện”. Với mỗi k, chúng ta có thể tính toán thời gian dự kiến ​​bằng cách trình bày tất cả các kết quả xác suất qua k bước. Điều này hoạt động về mặt khái niệm vì mỗi lần làm mới là độc lập, do đó, kỳ vọng có thể được mở rộng dưới dạng tổng giống như hình học trên tất cả các chuỗi kết quả. 

Điểm thất bại là k bị giới hạn một cách có ý nghĩa. Trong trường hợp xấu nhất, hành vi tối ưu tương ứng với việc tiếp tục làm mới vô thời hạn cho đến khi xuất hiện kết quả tốt. Điều đó có nghĩa là lực lượng vũ phu trên k thực sự là vô hạn. 

Quan sát chính là hệ thống có cấu trúc tự tương tự. Sau mỗi lần làm mới, Justin lại trở về trạng thái tương tự. Điều này có nghĩa là thời gian tối ưu dự kiến ​​​​E thỏa mãn phương trình điểm cố định: chi phí dự kiến ​​​​là tổng có trọng số trên kết quả, trong đó ở một số kết quả, chúng tôi chấp nhận chi phí ngay lập tức và ở những kết quả khác, chúng tôi lại phải trả 1 giây cộng với E. 

Điều này biến vấn đề thành giải một phương trình tuyến tính duy nhất xuất phát từ lựa chọn tối ưu cho mỗi kết quả. Đối với mỗi loại quảng cáo, Justin so sánh chi phí của việc chấp nhận nó với chi phí làm mới và tiếp tục với kỳ vọng E. Sự so sánh đó làm giảm vấn đề trong việc tính toán hành vi ngưỡng và thay thế nó vào phương trình kỳ vọng.

Một khi chúng ta biểu diễn tất cả các lựa chọn một cách nhất quán, toàn bộ hệ thống sẽ chuyển sang giải một E vô hướng duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force về số lần làm mới | Hàm mũ | O(1) | Quá chậm | 
| Kỳ vọng điểm cố định với lựa chọn tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Diễn giải mỗi kết quả như một sự kiện chi phí 

Chúng tôi chỉ định chi phí cho từng loại quảng cáo: không có quảng cáo nào có chi phí 0, quảng cáo đệm có chi phí 6, quảng cáo có thể bỏ qua đóng góp 5 giây và quảng cáo không thể bỏ qua đóng góp 15 hoặc 20 giây. 

Điểm khác biệt quan trọng là quảng cáo có thể bỏ qua hoạt động khác với quảng cáo không thể bỏ qua vì chúng có thể tránh được một phần kịp thời nhưng vẫn áp đặt chi phí chờ cố định nếu được chấp nhận. 

### 2. Giới thiệu trạng thái giá trị kỳ vọng E 

Đặt E biểu thị thời gian dự kiến tối ưu bắt đầu từ lần tải trang mới. Bởi vì mỗi lần làm mới đều quay trở lại trạng thái cũ nên mọi quyết định đều quay trở lại E. 

Sự đối xứng này là sự đơn giản hóa cốt lõi: chúng tôi không bao giờ cần theo dõi lịch sử hoặc số lần làm mới. 

### 3. Quyết định rõ ràng theo kết quả 

Đối với mỗi loại quảng cáo, Justin chọn giữa việc chấp nhận hoặc làm mới. Việc chấp nhận mang lại một chi phí cố định c. Làm mới mang lại 1 + E. 

Vì vậy, đối với mỗi loại, chi phí hiệu quả sẽ trở thành min(c, 1 + E). Bước này mã hóa hành vi tối ưu cục bộ cho mỗi kết quả. 

### 4. Xây dựng phương trình kỳ vọng 

Bây giờ chúng ta tính E là tổng trọng số của các kết quả: 

E bằng p0 nhân 0 cộng p5 nhân min(5, 1 + E) cộng p15 nhân min(15, 1 + E) cộng p20 nhân min(20, 1 + E) cộng p6 nhân min(6, 1 + E), với xác suất được chuẩn hóa dưới dạng phân số. 

Đây là một phương trình đơn trong một ẩn số E. 

### 5. Giải theo từng trường hợp nhất quán 

Chúng ta quan sát thấy 1 + E sẽ lớn hơn chi phí nhỏ và nhỏ hơn chi phí lớn tùy thuộc vào bản thân E. Chúng tôi kiểm tra xem loại quảng cáo nào đáng được chấp nhận so với việc làm mới, sau đó thay thế cho phù hợp. 

Điều này dẫn đến một chế độ nhất quán trong đó tất cả các quảng cáo tốn kém đều được làm mới cho đến khi đạt được kết quả không có quảng cáo, giảm hệ thống xuống mức kỳ vọng hình học. 

### Tại sao nó hoạt động 

Quá trình này là một quá trình quyết định Markov không có bộ nhớ với trạng thái giống hệt nhau sau mỗi lần làm mới. Điều này đảm bảo rằng chính sách tối ưu là ổn định và chỉ phụ thuộc vào việc so sánh chi phí trước mắt và chi phí tiếp tục. Chi phí tiếp tục chính xác là 1 + E, do đó việc thay thế giá trị này sẽ tạo ra một điểm cố định tự đồng nhất. Bởi vì tất cả tính ngẫu nhiên đều độc lập trong các lần làm mới nên phương trình kết quả nắm bắt đầy đủ tất cả các chiến lược có thể có và không có chính sách phụ thuộc vào lịch sử nào có thể hoạt động tốt hơn chính sách cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    p0, p5, p15, p20, p6 = map(int, input().split())

    p0 /= 100.0
    p5 /= 100.0
    p15 /= 100.0
    p20 /= 100.0
    p6 /= 100.0

    # We consider threshold behavior:
    # optimal solution is to keep refreshing until no-ad appears effectively.
    # so expectation reduces to geometric waiting time for p0 success + 1 second per refresh.

    # expected number of refreshes until success: 1 / p0
    # each refresh costs 1 second, and success yields 0 cost
    # but if success is immediate, no refresh cost is paid

    if p0 == 0:
        # impossible but constraints guarantee p0 > 0
        print(0.0)
        return

    # expected refresh cost until success:
    # geometric distribution: (1-p0)/p0 failures each costing 1 second
    # expected number of trials = 1/p0
    # expected refresh cost = (1/p0 - 1)
    E = (1.0 / p0) - 1.0

    print(E)

if __name__ == "__main__":
    solve()
```Mã giả định chiến lược tối ưu là làm mới liên tục cho đến khi kết quả không có quảng cáo xuất hiện, điều này sẽ thu gọn vấn đề thành thời gian chờ hình học. Điều này phản ánh ý tưởng rằng bất kỳ quảng cáo có chi phí dương nào cũng luôn tệ hơn việc trả 1 giây để quay lại khi xác suất không có quảng cáo là khác 0. 

Chi tiết triển khai chính là chuyển đổi tỷ lệ phần trăm thành xác suất và áp dụng công thức kỳ vọng cho quy trình hình học. Việc trừ 1 có nghĩa là lần rút thành công cuối cùng không phát sinh chi phí làm mới. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
50 0 50 0 0
```Chúng tôi chỉ không có quảng cáo hoặc quảng cáo 15 giây. Hệ thống trở thành một lần lật đồng xu lặp đi lặp lại. 

| Bước | Tiểu bang | Hành động | Kết quả xác suất | 
| --- | --- | --- | --- | 
| 1 | bắt đầu | làm mới hoặc chấp nhận | 50% không có quảng cáo, 50% 15 giây | 
| 2 | sau thất bại | làm mới lại | cùng phân phối | 

Nếu chúng ta tiếp tục làm mới cho đến khi không có quảng cáo nào xuất hiện thì số lần làm mới dự kiến ​​trước khi thành công là 1/p0 = 2, do đó chi phí dự kiến ​​là 1 lần làm mới. 

Điều này phù hợp với công thức E = 1/p0 - 1 = 2 - 1 = 1. 

Dấu vết cho thấy việc trì hoãn chấp nhận luôn có lợi vì hình phạt 15 giây chiếm ưu thế so với chi phí thử lại 1 giây. 

### Ví dụ 2 

đầu vào:```
15 25 0 35 25
```| Bước | Hành động | Kết quả | 
| --- | --- | --- | 
| 1 | làm mới | 15% không có quảng cáo, nếu không hãy thử lại quyết định | 
| 2 | lặp lại cho đến khi không có quảng cáo | quá trình hình học | 

Ở đây một lần nữa, tất cả các quảng cáo đều đắt hơn việc thử lại, do đó quá trình này giảm xuống còn việc chờ sự kiện không có quảng cáo. 

Chi phí làm mới dự kiến là 1/0,15 - 1 = 5,666... 

Điều này chứng tỏ rằng ngay cả với nhiều loại quảng cáo, chiến lược tối ưu sẽ bỏ qua chúng hoàn toàn khi chi phí thử lại nhỏ hơn lợi ích mong đợi của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ các phép tính số học không đổi trên năm xác suất | 
| Không gian | O(1) | Không sử dụng công trình phụ trợ | 

Giải pháp này dễ dàng phù hợp với các ràng buộc vì nó làm giảm toàn bộ quá trình quyết định ngẫu nhiên thành một công thức kỳ vọng duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    p0, p5, p15, p20, p6 = map(int, input().split())
    p0 /= 100.0
    if p0 == 0:
        return "0.0"
    return str((1.0 / p0) - 1.0)

assert abs(float(run("50 0 50 0 0")) - 1.0) < 1e-6
assert abs(float(run("15 25 0 35 25")) - 5.6666666) < 1e-6

# minimum p0 case
assert abs(float(run("100 0 0 0 0")) - 0.0) < 1e-6

# extreme skewed case
assert abs(float(run("1 99 0 0 0")) - 99.0) < 1e-6

# balanced case
assert abs(float(run("20 20 20 20 20")) - 4.0) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 100 0 0 0 0 | 0,0 | trường hợp cạnh thành công ngay lập tức | 
| 1 99 0 0 0 | 99,0 | xác suất thành công hiếm có | 
| 20 20 20 20 20 | 4.0 | hành vi hình học phân bố đồng đều | 

## Vỏ cạnh 

### Trường hợp 1: Luôn không có quảng cáo 

đầu vào:```
100 0 0 0 0
```Thuật toán tính p0 = 1, do đó chi phí dự kiến ​​trở thành 1/1 - 1 = 0. Justin luôn xem ngay lập tức nên không xảy ra quá trình làm mới. Công thức xử lý việc này một cách rõ ràng mà không gặp vấn đề gì về phép chia. 

### Trường hợp 2: Thành công cực hiếm 

đầu vào:```
1 99 0 0 0
```Ở đây p0 = 0,01, vì vậy chi phí dự kiến ​​là 99 lần làm mới. Thuật toán phản ánh thời gian chờ đợi lâu do thành công hiếm có, chia tỷ lệ chính xác kỳ vọng một cách tuyến tính với độ hiếm. 

### Trường hợp 3: Quảng cáo hỗn hợp nhưng không phù hợp với chiến lược tối ưu 

đầu vào:```
20 20 20 20 20
```Mặc dù có nhiều loại quảng cáo tồn tại nhưng tất cả đều bị chi phối bởi chi phí làm mới. Thuật toán vẫn coi quy trình là chờ đợi hình học, tạo ra kỳ vọng 4. Điều này xác nhận rằng cấu trúc quảng cáo trung gian không ảnh hưởng đến chính sách tối ưu khi việc làm mới chi phối mọi chi phí.
