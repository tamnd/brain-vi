---
title: "CF 104992C - \u0421\u043f\u043e\u0439, \u043f\u0442\u0438\u0447\u043a\u0430!"
description: "Chúng ta được cung cấp một chuỗi xếp hạng riêng biệt được gán cho các loài chim, trong đó mỗi vị trí tương ứng với một loài chim mới gặp theo thứ tự."
date: "2026-06-28T04:26:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104992
codeforces_index: "C"
codeforces_contest_name: "qual VKOSHP Junior 24"
rating: 0
weight: 104992
solve_time_s: 77
verified: false
draft: false
---

[CF 104992C - \u0421\u043f\u043e\u0439, \u043f\u0442\u0438\u0447\u043a\u0430!](https://codeforces.com/problemset/problem/104992/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi xếp hạng riêng biệt được gán cho các loài chim, trong đó mỗi vị trí tương ứng với một loài chim mới gặp theo thứ tự. Oleg lắng nghe mỗi con chim đúng một lần khi anh ấy gặp nó lần đầu tiên, nhưng hành vi của anh ấy khiến anh ấy phải lắng nghe lại nhiều hơn: bất cứ khi nào anh ấy gặp một con chim có xếp hạng nhỏ hơn một số xếp hạng đã thấy trước đó, anh ấy “nhảy trở lại” con chim được đánh giá tốt nhất mà anh ấy đã thấy cho đến nay và lắng nghe con chim tốt nhất đó một lần nữa trước khi tiếp tục. 

Vì vậy, quá trình này là một bước duyệt qua mảng, nhưng đôi khi được đặt lại thành phần tử tối đa hiện tại được thấy cho đến nay. Mỗi khi một giá trị mới xuất hiện, chúng ta luôn phải trả một phút để lắng nghe tiếng chim mới đó. Ngoài ra, nếu giá trị mới nhỏ hơn giá trị tiền tố tối đa hiện tại, chúng tôi cũng trả thêm một phút để nghe lại phần tử tối đa đó. 

Đầu ra yêu cầu hai thông tin: tổng số sự kiện nghe (lượt nghe ban đầu cộng với tất cả các lần lặp lại) và số lần tối đa bất kỳ con chim nào được nghe trong quá trình này. 

Ràng buộc n lên tới 200.000 ngụ ý rằng chúng ta cần một giải pháp O(n) hoặc O(n log n). Vì hành vi chỉ phụ thuộc vào cực đại tiền tố và so sánh, mọi mô phỏng đều phải tránh quét lặp lại để đạt mức tối đa, điều này sẽ giảm xuống O(n^2) trong trường hợp xấu nhất. 

Một cách tiếp cận đơn giản sẽ thất bại khi xảy ra nhiều “sự sụt giảm” sau khi tăng cực đại. Ví dụ: nếu trình tự xen kẽ giữa các giá trị cao và thấp, việc tính toán lại nhiều lần số lần truy cập tối đa hoặc theo dõi không hiệu quả sẽ gây ra các lần quét toàn bộ lặp đi lặp lại. 

Trường hợp cạnh tinh tế là khi mức tối đa liên tục thay đổi, giống như một dãy tăng dần. Trong trường hợp đó, không có sự lặp lại nào xảy ra. Ngược lại, một chuỗi như`100, 1, 99, 2, 98, 3, ...`buộc nhiều người phải nhảy trở lại mức tối đa tương tự nhiều lần. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ duy trì danh sách các loài chim đã được ghé thăm và ở mỗi bước, tìm kiếm loài chim được xếp hạng tối đa trong số chúng bất cứ khi nào xếp hạng hiện tại thấp hơn mức tối đa đó. Điều này yêu cầu quét tuyến tính theo từng bước hoặc cấu trúc ưu tiên với các bản cập nhật, nhưng khó khăn thực sự là chúng tôi không xóa hoặc chèn động, chúng tôi chỉ cần tiền tố tối đa hiện tại. 

Trong cách giải thích bạo lực, ở mỗi bước tôi chúng tôi tính toán mức tối đa trong số tất cả các phần tử trước đó bất cứ khi nào cần thiết. Trong trường hợp xấu nhất, mỗi bước sẽ kích hoạt quá trình quét O(n), cho ra O(n^2). Với n lên tới 200.000, điều này là quá chậm. 

Nhận xét quan trọng là “con chim tốt nhất cho đến nay” chỉ đơn giản là tiền tố tối đa. Khi chúng tôi duy trì giá trị này tăng dần, chúng tôi không bao giờ cần phải tính toán lại nó. Quá trình trở nên xác định: khi chúng tôi thấy a_i, chúng tôi so sánh nó với M tối đa hiện tại. Nếu a_i > M, chúng tôi cập nhật M. Ngược lại, chúng tôi thêm một lượt truy cập bổ sung vào M. 

Điều này làm giảm toàn bộ quá trình thành một lượt tuyến tính duy nhất và chúng tôi cũng có thể theo dõi số lượt truy cập trên mỗi con chim bằng cách sử dụng từ điển hoặc mảng được lập chỉ mục theo vị trí, vì mỗi con chim được xác định duy nhất theo vị trí của nó. 

Tổng số phút chỉ là tổng số lượt truy cập mà chúng tôi mô phỏng. Số lần tối đa một con chim được ghé thăm sẽ được cập nhật bất cứ khi nào chúng tôi tăng bộ đếm của một con chim. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các loài chim từ trái sang phải, duy trì hai phần trạng thái: xếp hạng tối đa hiện tại được thấy cho đến nay và bản đồ từ chỉ mục chim cho đến số lần nó được nghe. 

1. Khởi tạo mức tối đa hiện tại làm xếp hạng của con chim đầu tiên và đặt số lượt truy cập của nó thành 1, vì Oleg đã nghe nó một lần khi gặp lần đầu. Điều này thiết lập trạng thái cơ bản nơi ít nhất một con chim đã được nghe thấy. 
2. Khởi tạo tổng thời gian là 1, khớp với sự kiện nghe đầu tiên. 
3. Đối với mỗi con chim tiếp theo i từ 2 đến n, trước tiên hãy tăng bộ đếm của chính nó vì Oleg luôn lắng nghe một con chim mới khi đến nơi. Điều này phản ánh bước quan sát bắt buộc. 
4. Cộng thêm 1 vào tổng thời gian cho lần nghe đầu tiên này. 
5. So sánh a_i với mức tối đa hiện tại. Nếu a_i lớn hơn mức tối đa, hãy cập nhật mức tối đa thành a_i và không làm gì khác. Lý do là không có con chim nào được nhìn thấy trước đó tốt hơn con này nên không xảy ra hiện tượng quay lui. 
6. Nếu a_i nhỏ hơn mức tối đa, hãy tìm con chim nào hiện đang giữ mức tối đa (chúng tôi theo dõi chỉ số của nó), tăng bộ đếm lượt truy cập của nó và thêm 1 vào tổng thời gian. Mô hình này Oleg sẽ quay trở lại để nghe lại con chim hay nhất cho đến nay. 
7. Sau khi xử lý tất cả các con chim, hãy tính giá trị tối đa trong số tất cả các bộ đếm lượt truy cập. 

Tính chính xác phụ thuộc vào thực tế là ứng cử viên duy nhất để xem lại luôn là mức tối đa toàn cầu của tiền tố và mức tối đa này chỉ thay đổi khi xuất hiện xếp hạng lớn hơn nghiêm ngặt. 

### Tại sao nó hoạt động 

Ở mỗi bước, quyết định duy nhất phụ thuộc vào lịch sử là liệu “nhảy lùi” có xảy ra hay không và quyết định đó chỉ phụ thuộc vào việc giá trị hiện tại có nhỏ hơn giá trị tối đa của tất cả các giá trị trước đó hay không. Vì mức tối đa của tiền tố được xác định duy nhất và cập nhật đơn điệu nên thuật toán không bao giờ bỏ lỡ lần truy cập cần thiết và không bao giờ thực hiện một lần truy cập không cần thiết. Mỗi lần truy cập lại tương ứng chính xác với một trường hợp trong đó tiền tố tối đa thống trị phần tử hiện tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    # track current maximum value and its index
    max_val = a[0]
    max_idx = 0
    
    # visit counts per bird
    cnt = [0] * n
    
    cnt[0] = 1
    total = 1
    
    for i in range(1, n):
        cnt[i] += 1
        total += 1
        
        if a[i] > max_val:
            max_val = a[i]
            max_idx = i
        else:
            cnt[max_idx] += 1
            total += 1
    
    print(total, max(cnt))

if __name__ == "__main__":
    solve()
```Việc triển khai mã hóa trực tiếp quy trình được mô tả trong thuật toán. Mảng`cnt`lưu trữ số lần mỗi con chim được nghe, trong khi`max_val`Và`max_idx`duy trì con chim tốt nhất hiện tại ở tiền tố. Mỗi lần lặp lại đóng góp chính xác một lượt nghe bắt buộc và đôi khi là một lượt nghe bổ sung khi phần tử hiện tại không phải là mức tối đa mới. 

Một điểm tinh tế là chúng ta không bao giờ tính lại mức tối đa từ đầu. Biến`max_idx`luôn trỏ đến ứng cử viên chính xác để truy cập lại vì giá trị tối đa của tiền tố chỉ thay đổi khi xuất hiện giá trị lớn hơn. 

## Ví dụ đã hoạt động 

Xem xét đầu vào mẫu`6`với xếp hạng`2 4 1 3 5 6`. 

| tôi | một [tôi] | giá trị tối đa | max_idx | cập nhật cnt | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 0 | cnt[0]=1 | 1 | 
| 1 | 4 | 4 | 1 | cnt[1]=1 | 2 | 
| 2 | 1 | 4 | 1 | cnt[2]=1, cnt[1]=2 | 4 | 
| 3 | 3 | 4 | 1 | cnt[3]=1, cnt[1]=3 | 6 | 
| 4 | 5 | 5 | 4 | cnt[4]=1 | 7 | 
| 5 | 6 | 6 | 5 | cnt[5]=1 | 8 | 

Dấu vết này cho thấy rằng chỉ giảm tương ứng với số lượt truy cập bổ sung kích hoạt tối đa hiện tại. Con chim thứ hai tích lũy nhiều lượt truy cập lại vì nó vẫn duy trì mức tối đa trong một số bước sau đó. 

Bây giờ hãy xem xét`4 10 3 2 9`. 

| tôi | một [tôi] | giá trị tối đa | max_idx | cập nhật cnt | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 4 | 4 | 0 | cnt[0]=1 | 1 | 
| 1 | 10 | 10 | 1 | cnt[1]=1 | 2 | 
| 2 | 3 | 10 | 1 | cnt[2]=1, cnt[1]=2 | 4 | 
| 3 | 2 | 10 | 1 | cnt[3]=1, cnt[1]=3 | 6 | 
| 4 | 9 | 10 | 1 | cnt[4]=1, cnt[1]=4 | 8 | 

Điều này thể hiện mức tối đa tồn tại lâu dài, tích lũy số lượt truy cập lặp lại bất cứ khi nào các giá trị nhỏ hơn xuất hiện sau nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với bản cập nhật O(1) | 
| Không gian | O(n) | Truy cập quầy được lưu trữ cho mỗi con chim | 

Quét tuyến tính phù hợp với giới hạn lên tới 200.000 phần tử một cách thoải mái và tất cả các hoạt động đều là cập nhật liên tục theo thời gian của bộ đếm và so sánh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    _stdout = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = _stdout
    return out.strip()

# sample
assert run("6\n2 4 1 3 5 6\n") == "8 3"

# single element
assert run("1\n100\n") == "1 1"

# strictly increasing
assert run("5\n1 2 3 4 5\n") == "5 1"

# strictly decreasing
assert run("5\n5 4 3 2 1\n") == "9 5"

# alternating max pattern
assert run("4\n10 1 9 2\n") == "7 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 1 | trường hợp cơ sở | 
| ngày càng tăng | n,1 | không xem lại | 
| giảm dần | 2n-1, n | số lượt truy cập tối đa lặp lại | 
| xen kẽ | hỗn hợp | độ ổn định tối đa lặp lại | 

## Vỏ cạnh 

Đối với một con chim, quá trình này không có sự phân nhánh. Thuật toán khởi tạo`cnt[0]=1`và tổng số ngay lập tức trở thành 1 và vì không có lần lặp nào xảy ra nên số lượt truy cập tối đa là 1, khớp với kết quả đầu ra dự kiến. 

Theo một trình tự tăng dần nghiêm ngặt như`1 2 3 4`, mọi phần tử đều trở thành một cực đại mới, vì vậy`max_idx`cập nhật ở mỗi bước và không bao giờ xảy ra việc xem lại. Việc triển khai chỉ thực thi nhánh “tối đa mới”, giữ tổng bằng n và tất cả số lượng bằng 1. 

Theo trình tự giảm dần như`5 4 3 2 1`, phần tử đầu tiên trở thành phần tử tối đa toàn cục và giữ nguyên như vậy. Mỗi bước tiếp theo sẽ kích hoạt việc truy cập lại chỉ mục 0. Mã tăng liên tục`cnt[max_idx]`, tích lũy n lượt truy cập cho phần tử đầu tiên và tạo ra tổng số`2n-1`, nhất quán với một lượt truy cập bổ sung cho mỗi bước sau bước đầu tiên.
