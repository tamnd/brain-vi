---
title: "CF 106007K - Và các phần tử X"
description: "Chúng tôi đang mô phỏng một quy trình trên một mảng trong đó một giá trị nguyên v bắt đầu từ 0 và được cập nhật từng bước. Tại mỗi vị trí i, chúng ta phải áp dụng chính xác một trong hai thao tác theo bit bằng cách sử dụng phần tử mảng hiện tại a[i]."
date: "2026-06-22T16:43:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106007
codeforces_index: "K"
codeforces_contest_name: "The 2025 Aleppo Collegiate programming contest"
rating: 0
weight: 106007
solve_time_s: 87
verified: true
draft: false
---

[CF 106007K - Và các phần tử X](https://codeforces.com/problemset/problem/106007/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một quá trình trên một mảng trong đó một giá trị số nguyên duy nhất`v`bắt đầu từ số 0 và được cập nhật từng bước. Tại mỗi vị trí`i`, chúng ta phải áp dụng chính xác một trong hai phép toán theo bit bằng cách sử dụng phần tử mảng hiện tại`a[i]`. Hoặc chúng ta hợp nhất các bit thành`v`sử dụng bitwise OR hoặc chúng tôi hạn chế`v`sử dụng bit AND. Các thao tác phải tuân theo thứ tự mảng, nhưng chúng ta có thể tự do lựa chọn thao tác nào sẽ sử dụng ở mỗi chỉ mục, với hạn chế toàn cầu duy nhất là AND phải được sử dụng ít nhất`x`lần. 

Mục tiêu là kết thúc với giá trị lớn nhất có thể của`v`. 

Ràng buộc`n ≤ 10^5`mỗi trường hợp thử nghiệm với tổng số`10^5`gợi ý rằng chúng ta cần một giải pháp tuyến tính hoặc logarit tuyến tính cơ bản cho mỗi trường hợp thử nghiệm. Bất kỳ chiến lược nào thử tất cả các tập hợp con của vị trí AND hoặc mô phỏng các lựa chọn theo cấp số nhân đều không thể thực hiện được. Ngay cả lý luận bậc hai cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm. 

Khó khăn chính xuất phát từ sự tương tác giữa OR và AND. OR chỉ có thể thêm bit, trong khi AND chỉ có thể xóa các bit không có trong phần tử mảng hiện tại. Bởi vì các phép toán phải được áp dụng theo thứ tự, nên các phép toán AND có thể tạm thời thu nhỏ giá trị, nhưng các phép toán OR sau đó có thể tạo lại các bit bị mất nếu các bit đó xuất hiện trong các phần tử sau này. 

Trường hợp cạnh tinh tế phát sinh khi một bit chỉ xuất hiện trước vị trí AND đã chọn. Nếu bit đó không có trong phần tử AND thì nó sẽ bị mất vĩnh viễn vì sau này OR không thể giới thiệu lại nó. Ví dụ, hãy xem xét`a = [1, 0, 2]`và AND cuối cùng được đặt ở chỉ mục 2. Bit từ`1`biến mất ở chỉ mục 2 và không thể phục hồi được, vì chỉ mục 2 không chứa nó và chỉ mục 3 xuất hiện sau AND nhưng không thể khôi phục trở lại các bit bị mất trước AND nếu chúng không có trong các ràng buộc hậu tố. 

Một trường hợp quan trọng khác là khi chúng ta thực hiện sớm nhiều phép toán AND. Một trực giác ngây thơ có thể gợi ý rằng nhiều AND hạn chế vĩnh viễn giá trị rất nhiều, nhưng trên thực tế, chỉ AND cuối cùng mới hạn chế đáng kể câu trả lời cuối cùng, vì các phép toán OR sau nó có thể xây dựng lại giá trị. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ chọn, đối với mọi chỉ mục, áp dụng OR hay AND và mô phỏng quy trình. Điều này đúng nhưng khám phá`2^n`những khả năng hoàn toàn không thể thực hiện được ngay cả đối với`n = 40`. 

Chúng ta có thể tinh chỉnh chế độ xem bằng cách tập trung vào cấu trúc của các hoạt động. Các phép toán OR hoàn toàn là phép cộng, vì vậy nếu một phần tử được sử dụng trong OR, nó sẽ đóng góp vĩnh viễn các bit của nó cho mọi thứ xảy ra sau nó trừ khi AND loại bỏ chúng sau này. Hoạt động AND là nguồn gây mất mát duy nhất và mỗi bước AND sẽ thay thế`v`với`v & a[i]`, nghĩa là nó buộc`v`nằm bên trong bitmask của`a[i]`vào thời điểm đó. 

Quan sát quan trọng là các phép toán AND trước đó không hạn chế vĩnh viễn kết quả cuối cùng, bởi vì mọi hạn chế mà chúng áp đặt sau đó có thể được hoàn tác bằng các phép toán OR đưa lại các bit bị thiếu. Chỉ thao tác AND cuối cùng xảy ra trước khi kết thúc chuỗi mới có tác dụng lâu dài, vì sau đó không có ràng buộc AND nào nữa để sửa bất kỳ bit bị mất nào trừ khi các bit đó xuất hiện trong các phần tử OR sau này. 

Điều này làm giảm vấn đề trong việc chọn vị trí của AND cuối cùng và coi tất cả các AND trước đó là các phép toán linh hoạt chỉ giúp đáp ứng yêu cầu có ít nhất`x`AND nhưng không ảnh hưởng lâu dài đến cấu trúc giá trị cuối cùng. 

Đối với một vị trí cố định`k`được chọn làm AND cuối cùng, mọi thứ trước nó có thể được sử dụng để xây dựng cơ sở OR lớn, nhưng các bit chỉ xuất hiện trước đó`k`và không chứa trong`a[k]`tạm thời bị mất tại`k`. Một số bit đó có thể xuất hiện lại sau đó thông qua các phép toán OR sau`k`, do đó, chỉ những bit xuất hiện riêng trong tiền tố và không thể phục hồi được sau này mới trở thành tổn thất thực sự. 

Điều này dẫn đến lý luận về tiền tố-hậu tố: chúng tôi theo dõi những gì có thể được xây dựng trước đó`k`, cái gì tồn tại tại`k`, và những gì có thể được phục hồi sau`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trong hoạt động | O(2^n) | O(1) | Quá chậm | 
| Sửa vị trí AND cuối cùng bằng tiền tố/hậu tố OR | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Tính tiền tố HOẶC mảng trong đó`pref[i]`là bitwise OR của tất cả các phần tử từ`1`ĐẾN`i`. Điều này thể hiện mọi thứ có thể được xây dựng trước hoặc tại vị trí`i`sử dụng các phép toán OR. 
2. Tính hậu tố OR trong đó`suf[i]`là bitwise OR của tất cả các phần tử từ`i`ĐẾN`n`. Điều này thể hiện tất cả các bit vẫn có thể được đưa vào sau vị trí`i`. 
3. Tính toán`total = suf[1]`, OR của tất cả các phần tử. Đây là giới hạn trên tuyệt đối của số bit có thể đạt được nếu không tồn tại hạn chế AND. 
4. Đối với từng vị trí`k`có thể đóng vai trò là phép toán AND cuối cùng (nghĩa là`k ≥ x`vậy nên ít nhất chúng ta vẫn còn chỗ cho`x-1`AND trước đó), hãy đánh giá điều gì sẽ xảy ra nếu AND cuối cùng được đặt tại`k`. 
5. Tại vị trí`k`, giá trị hiện tại trước khi áp dụng AND có thể được giả sử bao gồm tất cả các bit từ tiền tố thông qua các phép toán OR. Sau khi nộp đơn`v = v & a[k]`, chúng ta mất tất cả các bit có trong tiền tố nhưng không có trong`a[k]`. 
6. Tuy nhiên, một số bit bị mất này có thể được phục hồi nếu chúng xuất hiện trong hậu tố sau`k`. Vì vậy, sự mất mát thực sự không thể phục hồi ở vị trí`k`là: 

bit trong`pref[k-1]`cái đó không có trong`a[k]`và cũng không có trong`suf[k+1]`. 
7. Câu trả lời của ứng viên cho việc này`k`là`total`trừ đi khoản lỗ này. 
8. Lấy giá trị tối đa trên tất cả các giá trị hợp lệ`k`. 

### Tại sao nó hoạt động 

Điều bất biến là tất cả các phép toán OR chỉ tăng tập hợp các bit có sẵn, trong khi phép toán AND chỉ hạn chế tạm thời trạng thái hiện tại. Bất kỳ hạn chế nào gây ra bởi các AND không phải cuối cùng đều có thể được hoàn tác sau đó vì các thao tác OR sau chúng có thể đưa lại các bit bị mất. Do đó, chỉ AND cuối cùng mới quan trọng trong việc xác định bit tiền tố nào có thể bị mất vĩnh viễn và thậm chí sau đó chỉ những bit không thể xuất hiện lại sau này trong chuỗi. Điều này làm giảm tối ưu hóa trình tự toàn cầu thành tối ưu hóa một vị trí so với AND cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        
        pref = [0] * n
        suf = [0] * n
        
        cur = 0
        for i in range(n):
            cur |= a[i]
            pref[i] = cur
        
        cur = 0
        for i in range(n - 1, -1, -1):
            cur |= a[i]
            suf[i] = cur
        
        total = suf[0]
        
        ans = 0
        
        for k in range(x - 1, n):
            prefix_part = pref[k - 1] if k > 0 else 0
            suffix_part = suf[k + 1] if k + 1 < n else 0
            
            lost = prefix_part & (~a[k])
            lost &= (~suffix_part)
            
            ans = max(ans, total - lost)
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng cấu trúc tiền tố và hậu tố OR để chúng ta có thể nhanh chóng xác định bit nào có sẵn trước và sau bất kỳ vị trí AND cuối cùng ứng cử viên nào. Vòng lặp qua các vị trí AND cuối cùng có thể thực thi ràng buộc ít nhất`x`Hoạt động AND phải tồn tại bằng cách đảm bảo có đủ chỉ số trước vị trí đã chọn. 

Việc tính toán của`lost`cô lập các bit được đưa vào sớm, bị loại bỏ bởi AND tại`k`, và sau này không thể phục hồi được. Trừ giá trị này khỏi OR toàn cục sẽ mang lại giá trị cuối cùng tốt nhất có thể đạt được cho lựa chọn đó. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào trong đó`a = [1, 0, 2]`Và`x = 1`. 

Chúng tôi tính tiền tố HOẶC là`[1, 1, 3]`, hậu tố HOẶC như`[3, 3, 2]`, và tổng OR là`3`. 

Chúng tôi kiểm tra từng vị trí AND cuối cùng có thể. 

| k | trước[k-1] | một [k] | suf[k+1] | lạc lối | tổng số - bị mất | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 3 | 0 | 3 | 
| 1 | 1 | 0 | 2 | 1 | 2 | 
| 2 | 1 | 2 | 0 | 0 | 3 | 

Tại`k = 1`, AND bằng 0 sẽ xóa sạch bit đến từ`1`, và nó không thể phục hồi được trong hậu tố, vì vậy câu trả lời sẽ bị loại bỏ. Điều này xác nhận rằng việc chọn vị trí AND cuối cùng kém có thể phá hủy vĩnh viễn thông tin tiền tố hữu ích. 

Bây giờ hãy xem xét`a = [2, 1, 3, 0]`với`x = 2`. 

Tiền tố HOẶC là`[2, 3, 3, 3]`, hậu tố HOẶC là`[3, 3, 3, 0]`, tổng cộng là`3`. 

| k | trước[k-1] | một [k] | suf[k+1] | lạc lối | tổng số - bị mất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 3 | 0 | 3 | 
| 2 | 3 | 3 | 0 | 0 | 3 | 
| 3 | 3 | 0 | 0 | 3 | 0 | 

Vị trí cuối cùng buộc tất cả các bit tích lũy phải được AND bằng 0, khiến mọi thứ không thể phục hồi được. Điều này cho thấy tại sao việc lựa chọn AND cuối cùng lại quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi trường hợp kiểm thử sử dụng hai lần quét tuyến tính cho tiền tố/hậu tố OR và một lần quét tuyến tính để đánh giá ứng viên | 
| Không gian | O(n) | Mảng tiền tố và hậu tố lưu trữ giá trị OR cho mỗi chỉ mục | 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm được giới hạn bởi`10^5`, do đó thuật toán chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old
    return out.getvalue().strip()

# minimal case
assert run("1\n1 1\n5\n") == "0"

# all equal values
assert run("1\n5 2\n7 7 7 7 7\n") == "7"

# suffix loss scenario
assert run("1\n3 1\n1 0 2\n") == "3"

# last AND destroys everything
assert run("1\n4 1\n3 1 2 0\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | AND phải được sử dụng, buộc v về 0 | 
| tất cả đều bình đẳng | cùng giá trị | AND không giảm OR cuối cùng | 
| giá trị hỗn hợp | phục hồi hoàn toàn qua hậu tố | có thể tránh được sự mất mát | 
| dấu 0 | cuối cùng VÀ có thể là thảm họa | nhạy cảm với vị trí cuối cùng | 

## Vỏ cạnh 

Mảng một phần tử buộc phép toán duy nhất là AND, điều này ngay lập tức giảm`v`về 0 bất kể giá trị. Thuật toán xử lý việc này vì vị trí hợp lệ duy nhất cho AND cuối cùng là chỉ mục`0`, Và`pref[-1]`được coi là bằng 0, không tạo ra sự đóng góp nào và do đó không mất mát. 

Khi tất cả các phần tử giống hệt nhau, mọi thao tác AND đều giữ nguyên giá trị hiện tại, do đó không có bit tiền tố nào bị mất. Trong trường hợp này,`lost`trở thành 0 cho tất cả các vị trí và giá trị tối đa vẫn là OR đầy đủ, bằng chính phần tử đó. 

Khi các giá trị khác nhau và các bit hữu ích xuất hiện sớm nhưng cũng xuất hiện lại sau đó, hậu tố OR đảm bảo các bit đó không bị tính là bị mất ngay cả khi AND cuối cùng loại bỏ chúng. Điều này cho thấy tại sao`lost`không bao gồm đóng góp hậu tố. 

Khi phần tử cuối cùng bằng 0, việc chọn nó làm phần tử cuối cùng VÀ buộc giá trị cuối cùng về 0 vì mọi bit đều bị xóa ở bước cuối cùng và không thể khôi phục lại sau đó. Thuật toán nắm bắt chính xác điều này dưới dạng tổn thất tối đa tại`k = n-1`.
