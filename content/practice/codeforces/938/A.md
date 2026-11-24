---
title: "CF 938A - Sửa lỗi từ"
description: "Nhiệm vụ mô tả một phép biến đổi trên một chuỗi trong đó các ký tự nhất định bị xóa theo quy tắc cục bộ liên quan đến nguyên âm. Chúng ta được cho một từ và liên tục sửa đổi nó cho đến khi không còn hai nguyên âm liền kề."
date: "2026-06-17T02:41:29+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 938
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 38 (Rated for Div. 2)"
rating: 800
weight: 938
solve_time_s: 81
verified: false
draft: false
---

[CF 938A - Sửa lỗi từ](https://codeforces.com/problemset/problem/938/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả một phép biến đổi trên một chuỗi trong đó các ký tự nhất định bị xóa theo quy tắc cục bộ liên quan đến nguyên âm. Chúng ta được cho một từ và liên tục sửa đổi nó cho đến khi không còn hai nguyên âm liền kề. Quy tắc sửa đổi rất cụ thể: bất cứ khi nào chúng ta thấy một nguyên âm có một nguyên âm khác ngay trước nó, chúng ta sẽ xóa nguyên âm trước đó. 

Một cách hữu ích để giải thích điều này là chuỗi được đơn giản hóa để các nguyên âm không bao giờ được phép xuất hiện liên tiếp. Tuy nhiên, quá trình này không phải là một quá trình dọn dẹp toàn cầu đơn giản. Nó mang tính tuần tự: việc xóa phụ thuộc vào tính liền kề được hình thành trước đó và việc xóa một ký tự có thể tạo ra các cặp nguyên âm liền kề mới cũng phải được xử lý. 

Đầu vào bao gồm một chuỗi các chữ cái Latinh viết thường. Đầu ra là dạng ổn định cuối cùng sau khi áp dụng tất cả các thao tác xóa như vậy. 

Vì độ dài chuỗi tối đa là 100 nên mọi mô phỏng tuyến tính hoặc bậc hai đều được chấp nhận. Một cách tiếp cận ngây thơ liên tục quét chuỗi và xóa các ký tự vẫn có thể vượt qua một cách thoải mái. Điều quan trọng hơn là xử lý tính chất động của vùng lân cận sau khi xóa. 

Một số trường hợp đặc biệt quan trọng: 

Trường hợp một cạnh là một chuỗi được tạo hoàn toàn bằng các nguyên âm như “aeiou”. Sau mỗi lần xóa, các hình thức liền kề mới và quá trình tiếp tục cho đến khi chỉ còn lại một nguyên âm. Việc triển khai bất cẩn chỉ loại bỏ lần xuất hiện đầu tiên của một cặp trong một lần chuyển sẽ dừng quá sớm. 

Một trường hợp khác là các mẫu xen kẽ như “abaeio”. Ở đây chỉ có vấn đề liên quan đến nguyên âm-nguyên âm, phụ âm đóng vai trò phân cách. Một cách tiếp cận sai lầm xử lý mọi nguyên âm lặp lại trên toàn cầu thay vì chỉ các vị trí liên tiếp sẽ loại bỏ các ký tự hợp lệ một cách không chính xác. 

Cuối cùng, các chuỗi có độ dài 1 hoặc 2 chấm dứt một cách bình thường hoặc chỉ yêu cầu một lần kiểm tra, nhưng chúng thường bộc lộ từng lỗi một trong quá trình quét logic. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu trực tiếp phản ánh quá trình chính xác như được mô tả. Chúng tôi liên tục quét chuỗi từ trái sang phải và bất cứ khi nào tìm thấy hai nguyên âm liên tiếp, chúng tôi sẽ xóa nguyên âm trước đó và bắt đầu quét lại. Điều này đúng vì nó tuân theo lời phát biểu vấn đề theo đúng nghĩa đen. 

Tuy nhiên, cách tiếp cận này có thể làm suy giảm hành vi bậc hai hoặc tệ hơn. Trong một chuỗi như “aaaaaa”, mỗi lần xóa sẽ dịch chuyển chuỗi đó và buộc phải quét lại, dẫn đến các thao tác khoảng O(n²). Mặc dù n ở đây nhỏ nhưng cách tiếp cận này trở nên kém hiệu quả về mặt khái niệm và khó lý giải hơn. 

Quan sát quan trọng là chúng ta không bao giờ cần phải khởi động lại quá trình quét từ đầu. Thay vào đó, chúng ta có thể xử lý chuỗi trong một lần truyền trong khi vẫn duy trì cấu trúc đại diện cho tiền tố hợp lệ hiện tại. Mỗi nhân vật mới chỉ tương tác với nhân vật được giữ cuối cùng. Nếu cả hai đều là nguyên âm thì chúng ta loại bỏ nguyên âm trước đó; nếu không chúng tôi nối thêm bình thường. 

Điều này biến vấn đề thành một phương pháp giảm phát trực tuyến cổ điển trong đó chúng tôi duy trì bộ đệm đầu ra giống như ngăn xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Được chấp nhận nhưng không hiệu quả | 
| Đường chuyền đơn dựa trên ngăn xếp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các ký tự từ trái sang phải trong khi duy trì danh sách kết quả ngày càng tăng.

1. Khởi tạo một danh sách trống sẽ lưu trữ các ký tự hợp lệ hiện tại. Điều này hoạt động như một ngăn xếp đại diện cho tiền tố đã sửa. 
2. Xác định điều kiện trợ giúp để kiểm tra xem ký tự có phải là nguyên âm hay không, trong đó nguyên âm là a, e, i, o, u, y. Sự phân loại này được cố định trong suốt thuật toán. 
3. Lặp lại từng ký tự trong chuỗi đầu vào theo thứ tự. 
4. Đối với mỗi ký tự, hãy kiểm tra xem danh sách kết quả có trống không và cả ký tự hiện tại và ký tự cuối cùng trong kết quả có phải là nguyên âm hay không. Nếu điều kiện này đúng thì có nghĩa là chúng ta có một cặp nguyên âm liên tiếp bị cấm. 
5. Nếu điều kiện đúng, hãy xóa ký tự cuối cùng khỏi danh sách kết quả. Chúng tôi chưa thêm ký tự hiện tại vì quy tắc chỉ định xóa nguyên âm trước đó. 
6. Sau khi xử lý khả năng loại bỏ, hãy thêm ký tự hiện tại vào danh sách kết quả. 
7. Tiếp tục cho đến khi tất cả các ký tự được xử lý. 
8. Nối danh sách kết quả thành một chuỗi và xuất ra. 

Lý do chúng tôi chỉ kiểm tra ký tự cuối cùng là vì tất cả cấu trúc trước đó đã được đảm bảo hợp lệ. Mọi vi phạm phải liên quan đến ký tự mới được thêm vào và ký tự còn tồn tại gần đây nhất. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý từng tiền tố của chuỗi, danh sách kết quả chứa một phiên bản của tiền tố đó không có nguyên âm liên tiếp. Mỗi bước bảo tồn thuộc tính này. Khi một ký tự mới được thêm vào, vi phạm mới duy nhất có thể xảy ra là giữa ký tự đó và ký tự được giữ cuối cùng. Nếu cả hai đều là nguyên âm, việc loại bỏ nguyên âm trước sẽ khôi phục tính hợp lệ mà không ảnh hưởng đến cấu trúc trước đó, vì các ký tự trước đó đã được xác thực và không thể tạo thành ký tự liền kề mới với ký tự hiện tại. 

Điều này đảm bảo rằng khi kết thúc quá trình quét, không có nguyên âm liên tiếp nào còn sót lại ở đầu ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

VOWELS = set("aeiouy")

def solve():
    n = int(input())
    s = input().strip()
    
    res = []
    
    for ch in s:
        if res and ch in VOWELS and res[-1] in VOWELS:
            res.pop()
        res.append(ch)
    
    print("".join(res))

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo chính xác cách diễn giải ngăn xếp của quy trình. Điều tinh tế quan trọng là sau khi bật một nguyên âm, chúng ta vẫn nối thêm ký tự hiện tại, vì quy tắc chỉ loại bỏ nguyên âm trước đó trong cặp chứ không phải nguyên âm hiện tại. 

Việc sử dụng danh sách làm ngăn xếp sẽ tránh việc tái cấu trúc chuỗi lặp đi lặp lại, điều này sẽ gây ra chi phí bậc hai. 

## Ví dụ đã hoạt động 

### Ví dụ 1: “kỳ lạ” 

Chúng tôi xử lý các ký tự một cách tuần tự. 

| Bước | Ký tự hiện tại | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | --- | 
| 1 | w | [] | nối thêm | w | 
| 2 | e | w | nối thêm | chúng tôi | 
| 3 | tôi | chúng tôi | e và i là nguyên âm, pop e, sau đó nối i | wi | 
| 4 | r | wi | nối thêm | wir | 
| 5 | d | wir | nối thêm | hàn | 

Dấu vết cho thấy chỉ có cặp nguyên âm “e i” mới kích hoạt việc xóa và cấu trúc sẽ ổn định một cách tự nhiên sau đó. 

### Ví dụ 2: “aeaa” 

| Bước | Ký tự hiện tại | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | --- | 
| 1 | một | [] | nối thêm | một | 
| 2 | e | một | a và e là nguyên âm, pop a, nối e | e | 
| 3 | một | e | e và a là nguyên âm, pop e, nối a | một | 
| 4 | một | một | a và a là các nguyên âm, pop a, nối a | một | 

Quá trình tiếp tục cho đến khi chỉ còn lại một nguyên âm duy nhất. Mỗi bước sẽ loại bỏ nguyên âm trước đó, tạo ra hiệu ứng rút gọn theo tầng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nhân vật được đẩy và bật tối đa một lần | 
| Không gian | O(n) | Ngăn xếp lưu trữ tối đa n ký tự | 

Các ràng buộc cho phép tối đa 100 ký tự, do đó, các giải pháp thậm chí ít tối ưu hơn cũng sẽ vượt qua. Cách tiếp cận tuyến tính được chọn vì nó phản ánh trực tiếp cấu trúc của quy trình và mở rộng quy mô một cách rõ ràng cho các đầu vào lớn hơn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    VOWELS = set("aeiouy")

    n = int(input())
    s = input().strip()
    
    res = []
    for ch in s:
        if res and ch in VOWELS and res[-1] in VOWELS:
            res.pop()
        res.append(ch)
    
    return "".join(res)

# provided sample
assert run("5\nweird\n") == "werd"

# all consonants
assert run("4\nbcdf\n") == "bcdf"

# all vowels cascade
assert run("3\naei\n") == "i"

# alternating vowels and consonants
assert run("6\naebaei\n") == "ebei"

# single character
assert run("1\na\n") == "a"

# repeated vowels
assert run("5\naaaaa\n") == "a"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| aei | tôi | xóa tầng | 
| bcdf | bcdf | không có trường hợp nguyên âm | 
| aaa | một | giảm nguyên âm lặp đi lặp lại | 
| aebaei | ebei | ổn định cấu trúc hỗn hợp | 

## Vỏ cạnh 

Đối với một chuỗi nặng hoàn toàn nguyên âm như “aeiouy”, thuật toán liên tục loại bỏ nguyên âm trước đó bất cứ khi nào có nguyên âm mới xuất hiện. Bắt đầu từ “a”, thêm “e” sẽ xóa “a” và giữ “e”, sau đó “i” xóa “e”, v.v. Kết quả cuối cùng chỉ là “y”, xác nhận tính bất biến rằng chỉ có một nguyên âm có thể tồn tại trong một chuỗi liên tiếp đầy đủ. 

Đối với đầu vào một ký tự, chẳng hạn như “a”, vòng lặp chạy một lần, ngăn xếp bắt đầu trống và ký tự được thêm trực tiếp. Không có sự so sánh nào xảy ra và kết quả đầu ra không thay đổi, chứng tỏ rằng các điều kiện biên không kích hoạt việc xóa không hợp lệ.
