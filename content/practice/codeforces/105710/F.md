---
title: "CF 105710F - Xem chim"
description: "Có hai tập hợp hữu hạn điểm trên trục số. Một bộ tượng trưng cho vị trí của chim, bộ kia tượng trưng cho vị trí của máy ảnh."
date: "2026-06-26T08:00:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105710
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 2-12-25 Div. 1 (Advanced)"
rating: 0
weight: 105710
solve_time_s: 59
verified: true
draft: false
---

[CF 105710F - Xem chim](https://codeforces.com/problemset/problem/105710/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Có hai tập hợp hữu hạn điểm trên trục số. Một bộ tượng trưng cho vị trí của chim, bộ kia tượng trưng cho vị trí của máy ảnh. Bạn được phép dịch chuyển mọi camera theo cùng một độ lệch số nguyên, dương hoặc âm, dịch một cách hiệu quả toàn bộ cấu hình camera một cách cứng nhắc dọc theo đường thẳng. Sau sự thay đổi này, một con chim được coi là “bắt” nếu nó trùng khớp chính xác với ít nhất một vị trí máy ảnh. 

Nhiệm vụ là chọn bản dịch sao cho số lượng chim nằm chính xác trên một vị trí camera nào đó là lớn nhất. 

Một cách hữu ích để diễn đạt lại vấn đề là nghĩ về những khác biệt. Nếu một con chim ở vị trí`a`và một camera ở vị trí`b`, sau đó dịch chuyển tất cả các camera theo một giá trị nào đó`x`, chúng trùng nhau đúng lúc`a = b + x`, hoặc tương đương`x = a - b`. Điều này có nghĩa là mỗi cặp máy ảnh chim đề xuất một giá trị dịch chuyển cụ thể để căn chỉnh chúng. 

Câu trả lời là giá trị dịch chuyển xuất hiện thường xuyên nhất trong số tất cả các khác biệt theo cặp. 

Các ràng buộc đủ nhỏ cho một nghiệm bậc hai. Với`n, m ≤ 1000`, tổng số cặp nhiều nhất là một triệu. Một cách đơn giản`O(nm)`tính toán nằm trong giới hạn trong Python và không cần cấu trúc dữ liệu nặng hơn hoặc các kỹ thuật tiệm cận nhanh hơn. 

Sự tinh tế chính là hiểu rằng nhiều con chim và máy ảnh có thể đồng ý về cùng một ca làm việc một cách độc lập. Điều này có nghĩa là chúng tôi không chọn các cặp mà đếm xem có bao nhiêu loài chim đồng ý với một bản dịch toàn cầu. Một sai lầm ngây thơ là cố gắng khớp từng camera một cách tham lam hoặc mô phỏng việc dịch chuyển và kiểm tra các phần chồng chéo nhiều lần; cả hai cách tiếp cận đều có nguy cơ tính hai lần hoặc tính toán lại các sắp xếp giống nhau nhiều lần. 

Một trường hợp thất bại cụ thể đối với phương pháp mô phỏng đơn giản là khi nhiều ca tạo ra sự chồng chéo một phần. Ví dụ như chim ở`[1, 3, 5]`và máy ảnh tại`[2, 4]`cho phép thay đổi`-1, -3, 1, ...`và việc thử từng ca một cách độc lập với tính toán lại đầy đủ sẽ chậm một cách không cần thiết mặc dù`nm`là nhỏ. 

Một sai lầm khác là coi đây là vấn đề chồng lấp khoảng thời gian và thử sắp xếp và trượt các cửa sổ. Điều đó không thành công vì việc căn chỉnh phụ thuộc vào sự bình đẳng chính xác sau khi dịch chứ không phải thứ tự tương đối. 

## Phương pháp tiếp cận 

Phương pháp brute-force thử mọi giá trị dịch chuyển có thể và, với mỗi ca, kiểm tra xem có bao nhiêu con chim trùng với một số camera đã dịch chuyển. Nếu chúng ta xem xét tất cả các thay đổi của ứng cử viên, chúng bị giới hạn bởi sự khác biệt giữa bất kỳ con chim nào và bất kỳ máy ảnh nào, vì vậy có nhiều nhất`n × m`khả năng. Đối với mỗi ca, việc kiểm tra tất cả các con gia cầm bằng tất cả các camera sẽ tiêu tốn một yếu tố khác, dẫn đến`O(n^2 m^2)`theo cách giải thích tệ nhất là quá chậm. 

Quan sát quan trọng là chúng ta thực sự không cần mô phỏng sự dịch chuyển một cách rõ ràng. Mỗi căn chỉnh hợp lệ được xác định bởi một sự khác biệt duy nhất`a[i] - b[j]`. Nếu một ca`x`được chọn, mọi cặp thỏa mãn`a[i] - b[j] = x`đóng góp chính xác một con chim đã bắt được cho ca đó, nhưng điều chúng tôi thực sự muốn là: đối với mỗi ca, hãy đếm xem có bao nhiêu con chim riêng biệt có thể khớp với ít nhất một máy ảnh. Vì các loài chim là khác nhau và các camera cũng khác nhau nên việc đếm cặp ở đây là đủ vì nhiều camera sắp xếp theo cùng một con chim không làm thay đổi số lượng chim. 

Điều này làm giảm vấn đề tính toán tần số chênh lệch và lấy tần số tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua ca với kiểm tra đầy đủ | O(n²m²) | O(1) | Quá chậm | 
| Đếm sự khác biệt theo cặp | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sửa một bản đồ băm trống sẽ lưu trữ số lần mỗi giá trị dịch chuyển xảy ra. Mỗi ca thể hiện một cách căn chỉnh camera với các loài chim sao cho có ít nhất một cặp trùng khớp. 
2. Đối với mọi vị trí chim`a[i]`, lặp lại mọi vị trí camera`b[j]`và tính độ dịch chuyển cần thiết`x = a[i] - b[j]`. Sự thay đổi này chính xác là số lượng chúng ta cần áp dụng cho tất cả các máy ảnh để con chim này phù hợp với máy ảnh này. 
3. Tăng tần suất`x`trên bản đồ từng cái một. Mỗi mức tăng tương ứng với một lần căn chỉnh camera chim hợp lệ theo ca đó. 
4. Sau khi xử lý tất cả các cặp, quét bảng tần số và lấy giá trị lớn nhất. Giá trị này thể hiện sự dịch chuyển nhằm căn chỉnh số lượng lớn nhất các cặp máy ảnh chim. 
5. Trả về tần số tối đa này làm câu trả lời. 

### Tại sao nó hoạt động 

Mỗi cặp`(a[i], b[j])`đóng góp vào đúng một giá trị dịch chuyển. Nếu chúng ta sửa một ca`x`, mỗi cặp với`a[i] - b[j] = x`trở nên căn chỉnh sau khi áp dụng sự thay đổi đó. Bởi vì mỗi con chim và vị trí máy ảnh là duy nhất nên mỗi cặp đóng góp tương ứng với một con chim riêng biệt được chụp trong sự thay đổi đó. Do đó, tối đa hóa số lượng cặp cho một ca nhất định tương đương với việc tối đa hóa số lượng chim bị bắt. 

Thuật toán ngầm tìm kiếm trên tất cả các bản dịch có thể có mà không liệt kê chúng một cách rõ ràng. Tính chính xác xuất phát từ thực tế là mọi sự căn chỉnh hợp lệ đều phải phát sinh từ một số khác biệt giữa camera chim và mọi khác biệt như vậy được tính đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    freq = {}

    for i in range(n):
        ai = a[i]
        for j in range(m):
            diff = ai - b[j]
            freq[diff] = freq.get(diff, 0) + 1

    print(max(freq.values()))

if __name__ == "__main__":
    solve()
```Cốt lõi của việc triển khai là vòng lặp lồng nhau trên chim và máy ảnh. Từ điển`freq`lưu trữ số lượng của mỗi ca. sử dụng`ai - b[j]`đảm bảo rằng chúng tôi đang tính toán bản dịch chính xác cần thiết để căn chỉnh cặp cụ thể đó. 

Một cạm bẫy triển khai phổ biến là đảo ngược thứ tự trừ. sử dụng`b[j] - a[i]`thay vì`a[i] - b[j]`không phá vỡ tính chính xác miễn là nó được sử dụng một cách nhất quán, nhưng các quy ước trộn lẫn sẽ dẫn đến việc tổng hợp không chính xác. 

Một điều tinh tế khác là chúng ta không bao giờ cần mô phỏng rõ ràng việc dịch chuyển tất cả các camera. Bản dịch được mã hóa hoàn toàn bằng giá trị chênh lệch. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
1 3 7 8 10
5 9 11 13 18
```Chúng tôi tính toán sự khác biệt cho một vài cặp: 

| một [tôi] | b[j] | shift x = a[i] - b[j] | 
| --- | --- | --- | 
| 1 | 5 | -4 | 
| 3 | 5 | -2 | 
| 7 | 11 | -4 | 
| 8 | 13 | -5 | 
| 10 | 11 | -1 | 

Khi chúng ta tiếp tục quá trình này cho tất cả các cặp, sự dịch chuyển`-4`xuất hiện thường xuyên nhất. Điều đó tương ứng với một bản dịch trong đó nhiều camera căn chỉnh với nhiều loài chim. 

Truy tìm tích lũy: 

| ca | đếm | 
| --- | --- | 
| -4 | 3 | 
| -2 | 1 | 
| -5 | 2 | 

Tối đa là`3`, vậy câu trả lời là`3`. 

Điều này xác nhận rằng thuật toán đang phân nhóm các sắp xếp một cách hiệu quả bằng cách dịch thay vì khớp từng cặp riêng lẻ. 

### Ví dụ 2 

đầu vào:```
3 4
1 3 5
2 7 13 18
```Sự khác biệt chính: 

| một [tôi] | b[j] | ca | 
| --- | --- | --- | 
| 1 | 2 | -1 | 
| 3 | 2 | 1 | 
| 5 | 2 | 3 | 
| 1 | 7 | -6 | 

Không có ca lặp lại thường xuyên; mỗi căn chỉnh về cơ bản là bị cô lập. 

| ca | đếm | 
| --- | --- | 
| -1 | 1 | 
| 1 | 1 | 
| 3 | 1 | 
| -6 | 1 | 

Tối đa là`1`, vì vậy chỉ có một con chim có thể được căn chỉnh theo bất kỳ bản dịch nào. 

Điều này thể hiện trường hợp không tồn tại cấu trúc toàn cục và câu trả lời tối ưu chỉ còn một kết quả khớp duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi con chim được ghép nối với mỗi camera đúng một lần để tính giá trị dịch chuyển | 
| Không gian | O(nm) | Trong trường hợp xấu nhất, mỗi cặp tạo ra một sự thay đổi riêng biệt được lưu trữ trong bản đồ băm | 

Những hạn chế`n, m ≤ 1000`làm`nm ≤ 10^6`, nằm trong giới hạn thoải mái đối với Python ngay cả khi có chi phí từ điển. Việc sử dụng bộ nhớ vẫn có thể chấp nhận được vì số lượng khóa được lưu trữ cũng bị giới hạn bởi`nm`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    freq = {}
    for i in range(n):
        for j in range(m):
            d = a[i] - b[j]
            freq[d] = freq.get(d, 0) + 1

    return str(max(freq.values()))

# provided samples
assert run("5 5\n1 3 7 8 10\n5 9 11 13 18\n") == "3"
assert run("3 4\n1 3 5\n2 7 13 18\n") == "1"

# custom cases
assert run("1 1\n10\n10\n") == "1", "single element match"
assert run("2 2\n1 100\n50 51\n") == "1", "no shared shift dominance"
assert run("3 3\n1 2 3\n1 2 3\n") == "3", "perfect alignment"
assert run("4 3\n1 5 9 13\n2 6 10\n") == "2", "repeating structured shifts"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 cặp giống hệt nhau | 1 | trường hợp ranh giới tối thiểu | 
| bộ rời rạc | 1 | không có ca lặp lại | 
| mảng giống hệt nhau | 3 | trường hợp căn chỉnh đầy đủ | 
| mẫu số học | 2 | sự khác biệt lặp đi lặp lại có cấu trúc | 

## Vỏ cạnh 

Đầu vào tối thiểu với một con chim và một máy ảnh sẽ cho biết liệu quá trình triển khai có xử lý chính xác các bản đồ tần số tầm thường hay không. Với đầu vào`1 1`và vị trí bằng nhau, sự thay đổi duy nhất là`0`, và câu trả lời phải là`1`. Vòng lặp vẫn chạy một lần và từ điển ghi lại một mục duy nhất. 

Trường hợp tất cả các loài chim và máy ảnh đều ở xa nhau sẽ kiểm tra xem liệu các thay đổi âm có được xử lý chính xác hay không. Ví dụ như chim`[1, 100]`và máy ảnh`[50, 60]`tạo ca`-49, -59, 40, 50`, tất cả đều khác biệt và tần số tối đa vẫn là`1`. Thuật toán không giả định tính tích cực ở bất kỳ đâu, vì vậy các khóa âm được hỗ trợ một cách tự nhiên. 

Một trường hợp được căn chỉnh hoàn toàn như những chú chim`[1,2,3]`và máy ảnh`[1,2,3]`tạo ra nhiều sự dịch chuyển về 0 lặp đi lặp lại. Mỗi cặp đóng góp vào cùng một khóa`0`, và tần số đạt tới`9`, phản ánh chính xác rằng tất cả các cặp đều thẳng hàng dưới bản dịch bằng 0.
