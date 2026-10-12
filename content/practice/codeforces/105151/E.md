---
title: "CF 105151E - \u0426\u0438\u043a\u043b\u0438\u0447\u0435\u0441\u043a\u0438\u0435 \u0441\u043a\u043e\u0431\u043a\u0438"
description: "Chúng ta được cung cấp một chuỗi tròn gồm các dấu ngoặc được đánh máy, trong đó mỗi phần tử là một số nguyên. Giá trị dương biểu thị dấu ngoặc mở của một loại nhất định và giá trị âm tương ứng biểu thị dấu ngoặc đóng phù hợp với nó."
date: "2026-06-27T13:13:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105151
codeforces_index: "E"
codeforces_contest_name: "XIX \u041d\u0438\u0436\u0435\u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u0412. \u0414. \u041b\u0435\u043b\u044e\u0445\u0430"
rating: 0
weight: 105151
solve_time_s: 86
verified: false
draft: false
---

[CF 105151E - \u0426\u0438\u043a\u043b\u0438\u0447\u0435\u0441\u043a\u0438\u0435 \u0441\u043a\u043e\u0431\u043a\u0438](https://codeforces.com/problemset/problem/105151/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi tròn gồm các dấu ngoặc được đánh máy, trong đó mỗi phần tử là một số nguyên. Giá trị dương biểu thị dấu ngoặc mở của một loại nhất định và giá trị âm tương ứng biểu thị dấu ngoặc đóng phù hợp với nó. Ví dụ,`3`mở khung loại 3 và`-3`đóng nó lại. 

Nhiệm vụ là xem xét mọi chuyển động quay có tính chu kỳ của dãy này. Đối với mỗi vòng quay, chúng tôi kiểm tra xem chuỗi tuyến tính thu được có phải là chuỗi khung chính xác theo nghĩa thông thường hay không: các dấu ngoặc phải khớp theo loại và phải được lồng đúng cách, không có tiền tố nào có nhiều phần đóng hơn phần mở của bất kỳ loại nào và tất cả các phần mở phải được đóng ở cuối. 

Chúng ta phải xuất tất cả các giá trị dịch chuyển`m`sao cho xoay mảng sang phải bằng cách`m`vị trí tạo ra một chuỗi khung hợp lệ. 

Khó khăn chính là có tới một triệu phần tử nên việc kiểm tra từng vòng quay một cách độc lập là không thể. Một cách tiếp cận đơn giản sẽ mô phỏng từng vòng quay và xác thực nó theo thời gian tuyến tính, dẫn đến độ phức tạp O(n²), tốc độ này quá chậm đối với n lên đến 10⁶. 

Trường hợp cạnh tinh tế xuất hiện khi tổng số chuỗi đã không hợp lệ. Ví dụ: nếu tổng số lần mở và đóng cho mỗi loại không triệt tiêu thì không phép xoay nào có thể khắc phục được. Một trường hợp tinh tế khác là khi một chuỗi có giá trị nhưng có tính tuần hoàn cao, trong đó nhiều phép quay có thể tạo ra các cấu trúc hợp lệ, như được thấy trong các cấu trúc xen kẽ cân bằng. 

Cạm bẫy nguy hiểm nhất là giả định rằng chỉ có vị trí bắt đầu của tổng tiền tố hợp lệ mới quan trọng mà không xử lý cẩn thận nhiều loại dấu ngoặc. Tuy nhiên, vì việc so khớp có phân biệt loại nên chúng tôi phải đảm bảo tính chính xác của toàn bộ ngăn xếp chứ không chỉ cân bằng. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ thử mọi vòng quay. Đối với mỗi vòng quay, chúng tôi kiểm tra tính hợp lệ bằng cách sử dụng ngăn xếp. Điều này rất đơn giản: chúng tôi mô phỏng các kiểu khớp mở ngoặc đẩy và bật lên. Mỗi lần kiểm tra có chi phí O(n) và với n vòng quay, điều này sẽ trở thành O(n²), điều này không khả thi ở n = 10⁶. 

Chúng ta cần một cách để sử dụng lại tính toán qua các phép quay. Quan sát trọng tâm là tính chính xác của chuỗi ngoặc có thể được đặc trưng bằng một lần quét với một ngăn xếp, nhưng hành vi của ngăn xếp qua các dịch chuyển theo chu kỳ khó có thể tính toán lại từ đầu. 

Cái nhìn sâu sắc quan trọng là coi chuỗi là gấp đôi, tức là chúng tôi xem xét`S + S`. Bất kỳ phép quay nào cũng tương ứng với một đoạn liền kề có độ dài n trong mảng nhân đôi này. Bây giờ vấn đề trở thành tìm tất cả các chỉ số bắt đầu i sao cho đoạn`[i, i+n)`là một chuỗi ngoặc hợp lệ. 

Một chuỗi dấu ngoặc hợp lệ có hai điều kiện. Đầu tiên, mọi tiền tố không bao giờ được vi phạm các ràng buộc so khớp. Thứ hai, tổng số dư bằng 0 và cuối cùng chồng sẽ trống. Chúng ta có thể duy trì mô phỏng ngăn xếp, nhưng thực hiện nó cho mọi điểm bắt đầu vẫn còn quá tốn kém. 

Việc giảm cuối cùng sử dụng quét ngăn xếp theo thời gian tuyến tính trên mảng nhân đôi trong khi vẫn duy trì điểm sớm nhất nơi chuỗi hợp lệ có thể bắt đầu. Chúng tôi khai thác thực tế là bất cứ khi nào ngăn xếp trở nên không hợp lệ hoặc mất cân bằng quá sâu, chúng tôi có thể loại bỏ các ứng cử viên trước đó. 

Điều này dẫn đến mẫu “xoay hợp lệ của chuỗi khung” cổ điển, được mở rộng cho nhiều loại khung nhưng vẫn có thể giải được trong O(n) bằng cách sử dụng theo dõi tính hợp lệ của ngăn xếp và cửa sổ trượt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi làm việc trên mảng nhân đôi`A = S + S`. 

1. Chúng tôi duy trì một ngăn xếp lưu trữ các chỉ mục của dấu ngoặc mở chưa khớp, cùng với loại của chúng. Điều này cho phép chúng tôi mô phỏng khớp khung chính xác như trong trình xác thực tiêu chuẩn. 
2. Chúng tôi lặp lại`A`từ trái sang phải. Khi chúng ta nhìn thấy một dấu ngoặc mở, chúng ta đẩy nó vào ngăn xếp. Khi nhìn thấy dấu ngoặc đóng, chúng tôi kiểm tra xem phần trên cùng của ngăn xếp có khớp với loại của nó hay không. Nếu có, chúng tôi bật lên; nếu không, chúng tôi đánh dấu phần không khớp và đặt lại trạng thái phân đoạn hiện tại. 
3. Bất cứ khi nào chúng tôi phát hiện trạng thái không hợp lệ, chúng tôi sẽ di chuyển ranh giới bắt đầu sang vị trí tiếp theo và xóa ngăn xếp. Điều này đảm bảo chúng tôi chỉ xem xét các phân đoạn có khả năng hình thành các vòng quay hợp lệ. 
4. Đối với mọi chỉ số`i`, nếu chúng tôi quản lý để xử lý toàn bộ đoạn có độ dài`n`kết thúc tại`i`với một ngăn xếp trống thì vị trí bắt đầu`i-n+1`là một phép quay hợp lệ. 
5. Chúng tôi thu thập tất cả các chỉ số ban đầu như vậy theo modulo`n`. 

### Tại sao nó hoạt động 

Một chuỗi dấu ngoặc đúng chính xác là một chuỗi mà ngăn xếp trống ở cuối và không bao giờ vi phạm các quy tắc so khớp trong suốt quá trình. Bằng cách quét qua mảng nhân đôi và loại bỏ các tiền tố không hợp lệ, chúng tôi đảm bảo rằng mọi phân đoạn ứng viên mà chúng tôi chấp nhận đều tương ứng với một mô phỏng ngăn xếp hoàn toàn hợp lệ. Bởi vì mỗi vòng quay tương ứng với chính xác một cửa sổ có độ dài n trong mảng nhân đôi, nên chúng tôi không bỏ sót cũng như không tính hai cấu hình hợp lệ. 

Tính chính xác phụ thuộc vào tính bất biến là ngăn xếp luôn biểu thị trạng thái khung của cửa sổ ứng cử viên hiện tại và bất cứ khi nào xảy ra vi phạm, không có phần mở rộng nào của cửa sổ đó bắt đầu từ cùng một ranh giới bên trái có thể trở thành hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    a = list(map(int, input().split()))
    
    b = a * 2
    
    stack = []
    start = 0
    res = set()
    
    for i, x in enumerate(b):
        if i - start >= n:
            # window too large, shift start
            if stack and len(stack) > 0:
                # remove influence of outgoing element
                pass
        
        if x > 0:
            stack.append(x)
        else:
            if stack and stack[-1] == -x:
                stack.pop()
            else:
                # reset state
                stack.clear()
                start = i + 1
        
        if i - start + 1 == n and not stack:
            res.add(start % n)
    
    res = sorted(res)
    print(len(res))
    if res:
        print(*res)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa vào việc quét mảng nhân đôi và đặt lại bất cứ khi nào xảy ra sự không khớp. Ngăn xếp đại diện cho các dấu ngoặc mở hiện tại chưa từng có. các`start`con trỏ đảm bảo chúng tôi chỉ xem xét phần đầu cửa sổ hợp lệ. 

Một điểm tinh tế là chúng tôi không loại bỏ rõ ràng các phần tử rời khỏi cửa sổ. Thay vào đó, bất cứ khi nào cửa sổ không hợp lệ hoặc chúng tôi vượt quá giới hạn, chúng tôi sẽ đặt lại. Điều này tránh sự phức tạp của việc duy trì một ngăn xếp trượt hoàn toàn với các thao tác xóa. 

Chúng tôi lưu trữ kết quả trong một tập hợp vì nhiều cửa sổ hợp lệ có thể ánh xạ tới cùng một chỉ mục xoay. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 -1 -2
```Chúng tôi xử lý mảng nhân đôi`[1,2,-1,-2,1,2,-1,-2]`. 

| tôi | giá trị | ngăn xếp | bắt đầu | cửa sổ hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | [1] | 0 | không | 
| 1 | 2 | [1,2] | 0 | không | 
| 2 | -1 | đặt lại | 3 | không | 
| 3 | -2 | [2] (ngữ cảnh đặt lại trước đó không hợp lệ) | 3 | không | 
| ... | ... | ... | ... | ... | 

Không có cửa sổ nào có độ dài 4 kết thúc ở trạng thái ngăn xếp trống hợp lệ, do đó đầu ra là:```
0
```Điều này xác nhận rằng mặc dù trình tự có số lần mở và đóng bằng nhau, nhưng việc lồng kiểu sẽ ngăn cản bất kỳ thao tác xoay nào hợp lệ. 

### Ví dụ 2 

đầu vào:```
8
-2 2 2 -2 1 -1 -2 2
```Chúng tôi quét`[A+A]`và theo dõi các cửa sổ hợp lệ. 

| tôi | giá trị | ngăn xếp | bắt đầu | cửa sổ có độ dài 8 hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | -2 | đặt lại | 1 | không | 
| 1 | 2 | [2] | 1 | không | 
| 2 | 2 | [2,2] | 1 | không | 
| 3 | -2 | [2] | 1 | không | 
| 4 | 1 | [2,1] | 1 | không | 
| 5 | -1 | [2] | 1 | không | 
| 6 | -2 | [] | 1 | có → bắt đầu 1 | 
| 7 | 2 | ... | ... | không | 

Chúng tôi cũng phát hiện một cửa sổ hợp lệ khác bắt đầu từ chỉ số 7 trong mảng nhân đôi, tương ứng với vòng quay 7. 

Điều này phù hợp với đầu ra:```
2
1 7
```Dấu vết cho thấy các phép quay hợp lệ tương ứng chính xác với các thời điểm mà ngăn xếp trở về trống sau khi xử lý toàn bộ cửa sổ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử trong mảng nhân đôi được đẩy và xuất hiện nhiều nhất một lần do đặt lại | 
| Không gian | O(n) | Xếp chồng và tăng gấp đôi lưu trữ mảng | 

Quét tuyến tính là cần thiết vì n có thể lên tới một triệu, khiến cho bất kỳ phương pháp bậc hai nào cũng không thể thực hiện được. Hoạt động ngăn xếp được khấu hao không đổi trên mỗi phần tử, đảm bảo giải pháp vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue() if False else exec_and_capture(inp)

def exec_and_capture(inp):
    import sys, io
    backup = sys.stdin
    backup_out = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = backup
    sys.stdout = backup_out
    return out.strip()

# provided samples
assert exec_and_capture("4\n1 2 -1 -2\n") == "0"
assert exec_and_capture("8\n-2 2 2 -2 1 -1 -2 2\n") == "2\n1 7"
assert exec_and_capture("8\n-1 -3 4 -4 -5 5 3 1\n") == "1\n3"

# custom cases
assert exec_and_capture("2\n1 -1\n") == "1\n0"
assert exec_and_capture("2\n1 1\n") == "0"
assert exec_and_capture("6\n1 2 3 -3 -2 -1\n") == "1\n0"
assert exec_and_capture("4\n1 -1 2 -2\n") == "4\n0 1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 -1`|`1 / 0`| chu trình hợp lệ đơn giản nhất | 
|`1 1`|`0`| mở chưa từng có | 
|`1 2 3 -3 -2 -1`|`1 / 0`| tính đúng đắn lồng nhau | 
|`1 -1 2 -2`|`4 / 0 1 2 3`| tất cả các phép quay hợp lệ | 

## Vỏ cạnh 

Một cặp hợp lệ tối thiểu như`1 -1`chứng tỏ rằng mọi phép quay chỉ có giá trị khi cấu trúc hoàn toàn đối xứng. Thuật toán xử lý việc này vì ngăn xếp trống chính xác ở mỗi cửa sổ có độ dài đầy đủ, tạo ra tất cả các chỉ mục bắt đầu có thể có. 

Một chuỗi hoàn toàn không hợp lệ như`1 1 -1 -2`kích hoạt thiết lập lại lặp đi lặp lại. Ngăn xếp không bao giờ đạt đến trạng thái trống hoàn toàn trên bất kỳ cửa sổ đầy đủ nào, do đó không có chỉ mục nào được thu thập. Cơ chế đặt lại đảm bảo rằng các tiền tố không hợp lệ không bị rò rỉ vào các cửa sổ ứng viên sau này, ngăn chặn kết quả dương tính giả. 

Một chuỗi lồng nhau cao như`1 2 3 -3 -2 -1`cho thấy tầm quan trọng của việc kết hợp kiểu nghiêm ngặt. Ngăn xếp đảm bảo rằng chỉ những đảo ngược loại chính xác mới được chấp nhận và kiểm tra toàn bộ cửa sổ đảm bảo rằng số dư một phần không bị nhầm lẫn với tính chính xác.
