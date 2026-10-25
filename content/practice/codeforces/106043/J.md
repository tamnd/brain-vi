---
title: "CF 106043J - Đá"
description: "Chúng ta được cấp một hàng đá, mỗi viên đá có một màu, được biểu thị bằng một chuỗi ký tự. Mục tiêu là biến đổi hàng sao cho không có hai viên đá liền kề nào có cùng màu. Chúng tôi không được phép sắp xếp lại các viên đá; thay vào đó, chúng ta có thể loại bỏ đá."
date: "2026-06-25T12:50:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106043
codeforces_index: "J"
codeforces_contest_name: "Teamscode Summer 2025 Advanced Division"
rating: 0
weight: 106043
solve_time_s: 49
verified: true
draft: false
---

[CF 106043J - Đá](https://codeforces.com/problemset/problem/106043/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng đá, mỗi viên đá có một màu, được biểu thị bằng một chuỗi ký tự. Mục tiêu là biến đổi hàng sao cho không có hai viên đá liền kề nào có cùng màu. Chúng tôi không được phép sắp xếp lại các viên đá; thay vào đó, chúng ta có thể loại bỏ đá. Nhiệm vụ là xác định số lần loại bỏ tối thiểu cần thiết để chuỗi còn lại không có ký tự bằng nhau liên tiếp. 

Nói cách khác, chúng tôi muốn nén chuỗi thành một chuỗi “liền kề xen kẽ chặt chẽ” bằng cách xóa càng ít ký tự càng tốt. Trình tự cuối cùng phải giữ nguyên thứ tự ban đầu, nhưng được phép xóa bất kỳ số lượng nào. 

Kích thước đầu vào trong vấn đề này nhỏ (phiên bản Codeforces điển hình có n lên đến 50), điều này ngay lập tức gợi ý rằng ngay cả quét bậc hai hoặc tuyến tính với các hằng số nhỏ cũng đủ. Bất cứ điều gì vượt quá O(n2) đều không cần thiết và thậm chí O(n2) là quá mức cần thiết. 

Các trường hợp cạnh khóa xuất hiện khi chuỗi đã hợp lệ hoặc khi tất cả các ký tự giống hệt nhau. 

Ví dụ: nếu đầu vào là`BRBG`, không có hai hàng xóm nào bằng nhau nên câu trả lời là 0. Nếu đầu vào là`RRRRR`, chúng ta chỉ được giữ lại một ký tự nên chúng ta loại bỏ 4 ký tự. 

Một dạng thất bại tinh vi đối với các phương pháp tiếp cận ngây thơ là cố gắng “mô phỏng việc loại bỏ” trong khi lặp lại mà không xử lý cẩn thận các chỉ số chuyển đổi. Ví dụ, trong`RBBR`, nếu bạn loại bỏ một cách tham lam mà không kiểm tra lại các cặp mới liền kề, bạn có thể xóa nhầm nhiều hơn mức cần thiết hoặc bỏ lỡ điều đó sau một lần xóa, cấu trúc sẽ thay đổi. 

Một giải pháp đúng sẽ tránh hoàn toàn việc mô phỏng và thay vào đó sẽ tính trực tiếp cấu trúc. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là cố gắng xóa các tập hợp con ký tự một cách rõ ràng và kiểm tra xem chuỗi kết quả có cặp liền kề bằng nhau hay không. Đối với mỗi tập hợp con, chúng tôi xây dựng lại chuỗi và xác thực nó trong O(n). Vì có 2ⁿ tập hợp con nên điều này nhanh chóng trở nên không khả thi ngay cả đối với n khiêm tốn; tại n = 50, giá trị này rất lớn về mặt thiên văn và hoàn toàn không thể sử dụng được. 

Quan sát quan trọng là chúng ta thực sự không cần phải quyết định những ký tự nào còn lại, chỉ có bao nhiêu cặp bằng nhau liên tiếp tồn tại trong chuỗi gốc. Mỗi khi chúng ta nhìn thấy hai ký tự liền kề giống hệt nhau thì ít nhất một trong số chúng phải bị loại bỏ. Nếu chúng ta quét từ trái sang phải và đếm xem có bao nhiêu vị trí tôi thỏa mãn s[i] == s[i−1], thì mỗi lần xuất hiện như vậy thể hiện một lần xóa bắt buộc trong một chiến lược tối ưu. 

Điều này có hiệu quả vì bất kỳ khối tối đa nào gồm các ký tự giống hệt nhau có độ dài k đều có thể được giảm xuống thành một ký tự đơn, yêu cầu xóa chính xác k−1. Tổng hợp điều này qua tất cả các lần chạy sẽ đưa ra câu trả lời. 

Điều này biến vấn đề từ một vấn đề lựa chọn tổ hợp thành một sự chuyển tuyến tính trên các chuỗi ký tự bằng nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(2ⁿ · n) | O(n) | Quá chậm | 
| Quét các ký tự bằng nhau | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu quét chuỗi từ trái sang phải, duy trì bộ đếm số lần xóa. 
2. Với mỗi vị trí i bắt đầu từ 1, so sánh ký tự hiện tại với ký tự trước đó. 
3. Nếu chúng bằng nhau, hãy tăng bộ đếm xóa vì một trong số chúng phải được loại bỏ để phá vỡ sự kề cận. 
4. Tiếp tục duyệt chuỗi mà không sửa đổi nó. 
5. Xuất ra tổng số lượng gia số sau khi quá trình quét kết thúc. 

Mỗi gia số tương ứng chính xác với một ký tự “phụ” bên trong một khối liền kề gồm các chữ cái giống hệt nhau, không thể duy trì trong chuỗi hợp lệ cuối cùng. 

### Tại sao nó hoạt động 

Chuỗi có thể được phân tách duy nhất thành các đoạn tối đa có ký tự bằng nhau. Trong một đoạn có độ dài k, bất kỳ chuỗi cuối cùng hợp lệ nào cũng có thể giữ lại tối đa một ký tự từ đoạn đó vì việc giữ hai ký tự sẽ ngay lập tức vi phạm quy tắc kề. Do đó số lần xóa tối thiểu cho phân đoạn đó là k−1. Tổng k−1 trên tất cả các phân đoạn tương đương với việc đếm các chuyển tiếp trong đó s[i] == s[i−1], đó chính xác là những gì thuật toán thực hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()

    deletions = 0

    for i in range(1, n):
        if s[i] == s[i - 1]:
            deletions += 1

    print(deletions)

if __name__ == "__main__":
    solve()
```Giải pháp dựa trên một lần quét tuyến tính duy nhất. Biến`deletions`theo dõi chính xác có bao nhiêu cặp bằng nhau liền kề tồn tại trong chuỗi gốc. Chúng tôi không bao giờ sửa đổi chuỗi để tránh các lỗi dịch chuyển chỉ mục thường gặp trong các phương pháp mô phỏng tham lam. 

Vòng lặp bắt đầu ở chỉ số 1 vì mỗi lần so sánh đều so sánh với ký tự trước đó. Điều này tránh việc kiểm tra ngoài giới hạn và nắm bắt rõ ràng mọi vùng lân cận một lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`RRG`| tôi | s[i-1] | s[i] | Bình đẳng? | xóa | 
| --- | --- | --- | --- | --- | 
| 1 | R | R | vâng | 1 | 
| 2 | R | G | không | 1 | 

Đầu ra cuối cùng là 1, nghĩa là chúng tôi loại bỏ một trong các chữ R liên tiếp. 

Điều này xác nhận rằng một lần chạy có độ dài 2 sẽ đóng góp chính xác một lần xóa. 

### Ví dụ 2:`RRRRR`| tôi | s[i-1] | s[i] | Bình đẳng? | xóa | 
| --- | --- | --- | --- | --- | 
| 1 | R | R | vâng | 1 | 
| 2 | R | R | vâng | 2 | 
| 3 | R | R | vâng | 3 | 
| 4 | R | R | vâng | 4 | 

Đầu ra cuối cùng là 4, phù hợp với ý tưởng rằng một khối có độ dài 5 cần 4 lần xóa. 

Ví dụ này cho thấy các phân đoạn thống nhất tích lũy các lần xóa một cách tuyến tính trong bao lâu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | truyền một lần qua chuỗi | 
| Không gian | O(1) | chỉ có một bộ đếm được lưu trữ | 

Các ràng buộc tuy nhỏ nhưng giải pháp tuyến tính vẫn rõ ràng nhất và tránh được logic mô phỏng không cần thiết. Ngay cả với n lớn hơn nhiều, điều này vẫn tối ưu và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    sys.stdout = out

    # solution
    n = int(input().strip())
    s = input().strip()

    deletions = 0
    for i in range(1, n):
        if s[i] == s[i - 1]:
            deletions += 1

    print(deletions)

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("3\nRRG\n") == "1", "sample 1"
assert run("5\nRRRRR\n") == "4", "sample 2"
assert run("4\nBRBG\n") == "0", "sample 3"

# custom cases
assert run("1\nR\n") == "0", "single element"
assert run("2\nRR\n") == "1", "minimal duplicate"
assert run("6\nRBBBRG\n") == "2", "mixed blocks"
assert run("7\nRGBRGBG\n") == "0", "already valid alternating"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 R`| 0 | trường hợp cạnh phần tử đơn | 
|`RR`| 1 | trường hợp loại bỏ nhỏ nhất | 
|`RBBBRG`| 2 | nhiều khối trùng lặp | 
|`RGBRGBG`| 0 | cấu trúc xen kẽ đã hợp lệ | 

## Vỏ cạnh 

Chuỗi ký tự đơn không chứa cặp liền kề, do đó quá trình quét không bao giờ tăng bộ đếm và đầu ra chính xác bằng 0. Thuật toán xử lý việc này một cách tự nhiên vì vòng lặp bắt đầu ở chỉ mục 1 và không bao giờ thực thi. 

Một chuỗi hoàn toàn thống nhất như`RRRRR`tạo ra một mức tăng ở mỗi bước, tích lũy chính xác n-1 lần xóa. Điều này phù hợp với yêu cầu chỉ có thể giữ lại một ký tự. 

Các chuỗi có cấu trúc xen kẽ như`RGBRGB`không tạo ra các cặp liền kề bằng nhau, do đó số lần xóa vẫn bằng 0, xác nhận rằng không có thao tác xóa không cần thiết nào được kích hoạt. 

Các chuỗi có khối hỗn hợp như`RBBR`cho thấy rằng chỉ có sự trùng lặp nội bộ mới quan trọng. Hai chữ B liên tiếp đóng góp chính xác một lần xóa và không có vị trí nào khác ảnh hưởng đến kết quả.
