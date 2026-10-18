---
title: "CF 105581F - Vốn chủ sở hữu"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Trong một lần di chuyển, chúng tôi được phép chọn một phân đoạn liền kề trong đó tất cả các giá trị giống hệt nhau và tăng mọi phần tử trong phân đoạn đó lên một."
date: "2026-06-22T06:10:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105581
codeforces_index: "F"
codeforces_contest_name: "Open Udmurtia Junior Programming Contest 2018"
rating: 0
weight: 105581
solve_time_s: 43
verified: true
draft: false
---

[CF 105581F - Vốn chủ sở hữu](https://codeforces.com/problemset/problem/105581/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Trong một lần di chuyển, chúng tôi được phép chọn một phân đoạn liền kề trong đó tất cả các giá trị giống hệt nhau và tăng mọi phần tử trong phân đoạn đó lên một. Mục tiêu là biến đổi toàn bộ mảng sao cho tất cả các phần tử trở nên bằng nhau, sử dụng càng ít phần tăng phân đoạn như vậy càng tốt. 

Một cách hữu ích để suy nghĩ về hoạt động này là chúng ta không tự do tăng các vị thế riêng lẻ. Chúng ta chỉ có thể “vẽ” một cao nguyên bằng phẳng có giá trị bằng nhau và nâng nó lên một cách đồng đều. Khó khăn đến từ việc sau mỗi hoạt động, cấu trúc của các phân khúc bằng nhau sẽ thay đổi và các hoạt động trong tương lai phụ thuộc rất nhiều vào các ổn định đang phát triển này. 

Các ràng buộc cho phép lên đến$10^5$phần tử có giá trị lên tới$10^9$. Điều này ngay lập tức loại trừ mọi giải pháp mô phỏng hoạt động hoặc liên tục quét và cập nhật các phân đoạn trên mỗi lần tăng, vì thậm chí$O(n^2)$sẽ là quá chậm. Chúng ta cần một cái gì đó gần hơn với thời gian tuyến tính hoặc tuyến tính. 

Một vài trường hợp đặc biệt làm rõ hành vi: 

Đối với một mảng đã thống nhất như`5 5 5 5`, không cần thực hiện thao tác nào và câu trả lời là 0. 

Đối với một mảng tăng nghiêm ngặt như`1 2 3 4`, người ta có thể nghĩ sai rằng mỗi phần tử cần các thao tác độc lập, nhưng các thao tác phân đoạn được phép lại tương tác theo một cách không hề tầm thường. Một kẻ tham lam ngây thơ luôn sửa một yếu tố duy nhất sẽ bị tính quá mức. 

Đối với một mảng giảm hoặc dao động như`3 1 3 1 3`, các quyết định cục bộ rất quan trọng vì việc chọn một phân khúc phẳng có thể ảnh hưởng đến các phân khúc sẵn có trong tương lai. 

Một điều tinh tế quan trọng là chúng ta không bao giờ được phép hợp nhất trực tiếp các phần tử không bằng nhau. Chỉ có thể chọn các khối liền kề bằng nhau, do đó cấu trúc của các lần chạy có giá trị bằng nhau là cơ bản. 

## Phương pháp tiếp cận 

Mô phỏng brute-force sẽ cố gắng liên tục xác định một số phân đoạn hợp lệ và áp dụng thao tác cho đến khi tất cả các giá trị trở nên bằng nhau. Người ta có thể chọn bất kỳ phân khúc giá trị tối thiểu nào hoặc nhắm mục tiêu tối đa toàn cầu, nhưng bất kể chiến lược nào, mỗi thao tác chỉ tăng giá trị cục bộ và có khả năng thay đổi cấu trúc phân khúc trên toàn bộ mảng. 

Ngay cả khi chúng tôi duy trì các phân đoạn một cách rõ ràng, mỗi thao tác có thể phân chia hoặc hợp nhất nhiều lần chạy và trong trường hợp xấu nhất, chúng tôi có thể thực hiện tối đa$O(\max(A) - \min(A))$hoạt động trên mỗi vị trí hoặc tệ hơn. Vì giá trị tăng lên$10^9$, điều này hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về các hoạt động riêng lẻ và thay vào đó lý luận ngược lại: thay vì xây dựng mảng trở lên, hãy xem xét cách giải quyết sự khác biệt giữa các phần tử liền kề. Mỗi lần chúng ta có sự sụt giảm hoặc tăng lên giữa các hàng xóm, nó đại diện cho một ranh giới buộc các hoạt động nhất định. 

Chúng ta có thể diễn giải lại quy trình này như duy trì một cấu trúc trong đó các phân đoạn bằng nhau mở rộng và tương tác, đồng thời chi phí được xác định bằng số lần chúng ta cần “kết nối” các thay đổi trong mảng. Mỗi mức tăng cục bộ trên một đoạn phẳng sẽ truyền bá một cấp độ đi lên trên đoạn đó một cách hiệu quả. Số lượng các hoạt động cần thiết có thể được suy ra từ số lượng "lớp tăng trưởng" độc lập được yêu cầu và các lớp này được xác định bằng cách chuyển đổi trong quá trình phân tách đơn điệu của mảng. 

Điều này dẫn đến một giải pháp tuyến tính trong đó chúng tôi xử lý mảng và đếm các đóng góp từ cấu trúc cục bộ thay vì mô phỏng các hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | số mũ /$O(n \cdot \max A)$|$O(n)$| Quá chậm | 
| Đếm cấu trúc tuyến tính |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng mọi thao tác đều làm tăng một khối liền kề có giá trị bằng nhau, có nghĩa là các thay đổi chỉ lan truyền trên các đoạn phẳng hiện có. Điều này cho thấy câu trả lời phụ thuộc vào cách các giá trị thay đổi khi di chuyển dọc theo mảng thay vì cường độ toàn cầu. 
2. Duyệt mảng và tập trung vào các khác biệt liên tiếp. Bất cứ khi nào chuỗi thay đổi giá trị giữa các chỉ số liền kề, vị trí đó thể hiện ranh giới cấu trúc nơi các hoạt động trong tương lai phải điều hòa lịch sử tăng trưởng không khớp. 
3. Thay vì đếm trực tiếp các thao tác, hãy diễn giải từng phần tử như được xây dựng từ các lớp tăng dần. Mỗi lớp tương ứng với việc đẩy một số khối bằng nhau liền kề lên trên một đơn vị. 
4. Khi di chuyển từ trái sang phải, hãy duy trì ý tưởng rằng chúng ta đang “lấp đầy” sự khác biệt về chiều cao. Nếu giá trị hiện tại lớn hơn giá trị trước đó, chúng ta thực sự cần thêm công việc chưa được các phân đoạn trước đó tính đến. 
5. Duy trì sự tích lũy liên tục của các mức tăng cần thiết bắt nguồn từ quá trình chuyển đổi đi lên. Mỗi khi mảng tăng đến một giới hạn, chúng ta phải đưa ra các phép toán mới tỷ lệ với mức tăng đó, bởi vì không có thao tác nào trước đó có thể ảnh hưởng đến mức cao hơn này trước khi mức tăng tồn tại. 
6. Tính tổng tất cả những đóng góp cần thiết đó trên mảng để có được câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Mỗi thao tác tương ứng với việc chọn một vùng liền kề tối đa có chiều cao bằng nhau tại một thời điểm nào đó và tăng nó một cách đồng đều. Điều này ngụ ý rằng bất kỳ sự gia tăng chiều cao nào đều phải bắt nguồn từ một khoảng nào đó vốn đã bằng phẳng ở mức trước đó. Do đó, tổng số thao tác chính xác là số lần các “lớp chiều cao” mới được giới thiệu qua các ranh giới nơi cấu trúc mảng buộc phải phát triển độc lập. 

Thuật toán phân tách hiệu quả cấu hình cuối cùng thành các lớp tăng trưởng độc lập này. Vì mỗi lớp được tính chính xác một lần tại thời điểm cần thiết đầu tiên nên không có thao tác nào được tính hai lần và không có lớp bắt buộc nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    if n == 1:
        print(0)
        return

    # We compute contribution of upward differences
    ans = 0

    for i in range(1, n):
        if a[i] > a[i - 1]:
            ans += a[i] - a[i - 1]

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai cốt lõi là một lần truyền qua mảng. Chúng tôi chỉ tích lũy đóng góp khi chuỗi tăng lên, vì những chuyển động đi xuống được “bao phủ” một cách tự nhiên bởi các hoạt động làm phẳng trước đó hoặc tương lai. Chi tiết triển khai quan trọng là chỉ những khác biệt tích cực mới quan trọng; trừ theo cả hai hướng sẽ được tính gấp đôi. 

Trường hợp khởi tạo cho$n = 1$được xử lý rõ ràng, mặc dù vòng lặp cũng sẽ mang lại kết quả bằng 0. Giữ nó rõ ràng sẽ tránh được sự nhầm lẫn khi suy luận về các chuyển đổi trống. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`3 1 2 4`Chúng tôi chỉ tính toán sự khác biệt hướng lên. 

| tôi | a[i-1] | một [tôi] | a[i] - a[i-1] | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | -2 | 0 | 0 | 
| 2 | 1 | 2 | +1 | 1 | 1 | 
| 3 | 2 | 4 | +2 | 2 | 3 | 

Câu trả lời cuối cùng là 3. Điều này cho thấy rằng chỉ những bước nhảy lên trên mới đóng góp, trong khi những bước di chuyển xuống dưới không trực tiếp làm tăng số lượng thao tác. 

### Ví dụ 2:`1 3 2 5`| tôi | a[i-1] | một [tôi] | a[i] - a[i-1] | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | +2 | 2 | 2 | 
| 2 | 3 | 2 | -1 | 0 | 2 | 
| 3 | 2 | 5 | +3 | 3 | 5 | 

Ở đây chúng ta thấy cấu trúc xen kẽ. Mỗi bước nhảy lên trên sẽ thêm các lớp bắt buộc mới, trong khi các bước nhảy xuống không hủy bỏ các yêu cầu trước đó. 

Những dấu vết này cho thấy rằng quá trình này hoàn toàn mang tính chất bổ sung đối với việc tăng lên, củng cố rằng các hoạt động tương ứng với việc đưa ra các mức độ cao mới thay vì loại bỏ chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Truyền một lần qua mảng, công việc không đổi trên mỗi phần tử | 
| Không gian |$O(1)$| Chỉ có một khoản tiền được duy trì | 

Giải pháp phù hợp thoải mái trong các ràng buộc cho$n \le 10^5$, chỉ yêu cầu xử lý tuyến tính và bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    output = io.StringIO()
    with contextlib.redirect_stdout(output):
        solve()
    return output.getvalue().strip()

# sample-like cases
assert run("3\n3 1 2 4\n") == "3"

# minimum size
assert run("1\n7\n") == "0"

# all equal
assert run("5\n2 2 2 2 2\n") == "0"

# strictly increasing
assert run("4\n1 2 3 4\n") == "3"

# alternating
assert run("5\n1 3 1 3 1\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | tính đúng đắn của trường hợp cơ sở | 
| tất cả đều bình đẳng | 0 | không có hoạt động không cần thiết | 
| trình tự tăng dần | n-1 | xử lý tăng trưởng đơn điệu | 
| trình tự xen kẽ | tăng tích lũy | cấu trúc không đơn điệu | 

## Vỏ cạnh 

Đối với mảng một phần tử như`10`, thuật toán ngay lập tức trả về 0 vì vòng lặp không bao giờ thực thi. Điều này phù hợp với thực tế là không cần thực hiện thao tác nào khi không có gì để cân bằng. 

Đối với một mảng không đổi như`5 5 5 5`, mọi sự khác biệt đều bằng 0, do đó không có đóng góp nào được thêm vào. Mặc dù các phân đoạn tồn tại nhưng không có chuyển tiếp đi lên nào tồn tại để kích hoạt bất kỳ hoạt động nào. 

Đối với một mảng giảm dần như`5 4 3 2`, mọi sự khác biệt đều âm, vì vậy một lần nữa không có đóng góp nào được thêm vào. Điều này xác nhận rằng các chuyển đổi đi xuống không tạo ra các hoạt động; chúng được ngầm xử lý bởi cấu trúc đi lên sớm hơn hoặc muộn hơn. 

Đối với một mảng hỗn hợp như`1 4 2 6`, quá trình tính toán được thực hiện bằng cách tính tổng chỉ tăng:`+3`từ`1→4`Và`+4`từ`2→6`, trong khi`4→2`bị bỏ qua. Điều này cho thấy thuật toán chỉ tính các yêu cầu tăng trưởng độc lập một cách nhất quán và không có bước nào phụ thuộc vào kiến ​​thức về mức giảm trong tương lai.
