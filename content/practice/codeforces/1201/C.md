---
title: "CF 1201C - Trung vị tối đa"
description: "Chúng ta được cung cấp một danh sách các số và được phép tăng liên tục bất kỳ phần tử đơn lẻ nào lên một. Mỗi mức tăng như vậy có chi phí cho một hoạt động và chúng tôi có tổng ngân sách tối đa là $k$ hoạt động."
date: "2026-06-13T15:12:32+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1201
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 577 (Div. 2)"
rating: 1400
weight: 1201
solve_time_s: 239
verified: true
draft: false
---

[CF 1201C - Trung vị tối đa](https://codeforces.com/problemset/problem/1201/C) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, tham lam, toán học, sắp xếp 
**Thời gian giải:** 3 phút 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách các số và được phép tăng liên tục bất kỳ phần tử đơn lẻ nào lên một. Mỗi mức tăng như vậy có chi phí cho một hoạt động và chúng tôi có tổng ngân sách tối đa là$k$hoạt động. Mục tiêu không phải là tối đa hóa tổng hoặc bất kỳ thống kê tổng thể nào mà cụ thể là tối đa hóa giá trị trung vị sau khi mảng được sắp xếp. 

Vì kích thước mảng là số lẻ nên trung vị là phần tử có vị trí cuối cùng$n/2$sau khi sắp xếp. Khó khăn chính là khi chúng ta tăng một số phần tử lên thì thứ tự sắp xếp có thể thay đổi nên số trung vị không bị ràng buộc với một chỉ số cố định trong mảng ban đầu. 

Các ràng buộc đủ lớn để bất kỳ chiến lược nào mô phỏng từng hoạt động một đều không thể thực hiện được. Với$n$lên đến$2 \cdot 10^5$Và$k$lên đến$10^9$, thậm chí một vòng lặp hoạt động có độ dài$k$rõ ràng sẽ vượt quá giới hạn thời gian. Bất kỳ giải pháp khả thi nào cũng phải giảm vấn đề thành sắp xếp cộng với xử lý tuyến tính hoặc tìm kiếm logarit trên câu trả lời. 

Một chế độ thất bại tinh vi xuất hiện khi tham lam nghĩ đến việc trực tiếp tăng vị trí trung vị. Nếu chúng ta chỉ tăng phần tử trung vị hiện tại, chúng ta sẽ bỏ qua thực tế là các phần tử khác ở bên phải của nó cũng có thể cần được tăng lên để duy trì trạng thái trung vị của nó sau khi sắp xếp. Ví dụ: nếu chúng ta chỉ tăng số trung vị một cách cô lập, thì một phần tử nhỏ hơn ở bên phải của nó vẫn có thể chặn nó trở thành số trung vị sau khi sắp xếp. 

Sai lầm thứ hai phát sinh nếu chúng ta cố gắng duy trì cấu trúc được sắp xếp động và liên tục điều chỉnh giá trị trung vị. Việc này trở nên quá chậm do phải chèn và xóa nhiều lần, đồng thời cũng che khuất cấu trúc thực sự của vấn đề: chỉ các phần tử ở hoặc trên vị trí trung vị mới quan trọng đối với câu trả lời cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng quá trình áp dụng các phép toán trong tất cả các phân bố có thể có giữa các phần tử. Sau mỗi lần tăng, chúng tôi sẽ sử dụng mảng và theo dõi giá trị trung bình. Ngay cả khi chúng tôi hạn chế chỉ phân phối số tiền tăng thêm cho các yếu tố có triển vọng thì số lượng phân phối có thể tăng lên theo cách kết hợp với$k$, làm cho phương pháp này không thể thực hiện được. 

Quan sát quan trọng là sau khi mảng được sắp xếp, trung vị nằm ở một vị trí cố định và chỉ các phần tử từ vị trí đó trở đi mới có thể ảnh hưởng đến mức độ lớn của mảng. Bất kỳ phần tử nào ở bên trái của số trung vị không hạn chế giá trị trung vị vì việc tăng chúng chỉ giúp ích cho số trung vị hoặc giữ nguyên giá trị đó sau khi sắp xếp lại. 

Điều này dẫn đến một cấu trúc tham lam: chúng ta muốn nâng đường trung bình và mọi thứ về bên phải của nó để đường trung bình có thể được đẩy lên cao nhất có thể. Nếu chúng ta cố định một giá trị đích cho số trung vị, chúng ta có thể kiểm tra xem cần bao nhiêu thao tác để làm cho tất cả các phần tử ở nửa bên phải ít nhất có giá trị đó. Điều này tự nhiên dẫn đến việc kiểm tra tính khả thi. 

Từ đây, tìm kiếm nhị phân trở thành công cụ tự nhiên. Chúng tôi tìm kiếm giá trị trung bình tối đa sao cho có thể nâng trung vị và hậu tố của mảng lên mức đó trong phạm vi ngân sách$k$. Mỗi kiểm tra là tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong$k$|$O(n)$| Quá chậm | 
| Tối ưu (tìm kiếm nhị phân + kiểm tra tham lam) |$O(n \log (k + \max a))$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta sắp xếp mảng sao cho vị trí trung vị cố định. Cho phép$m = n/2$. Từ thời điểm này, trung vị chúng tôi kiểm soát là chỉ số$m$và chỉ các chỉ số$m, m+1, \ldots, n-1$vấn đề để thực thi một giá trị trung bình ứng cử viên. 

1. Chúng ta sắp xếp mảng vì bất kỳ lý do nào về số trung vị đều yêu cầu một thứ tự ổn định. Nếu không sắp xếp, chúng ta không thể xác định được phần tử nào hạn chế số trung vị. 
2. Chúng ta định nghĩa một hàm kiểm tra xem một giá trị đã cho có$x$có thể đạt được ở mức trung bình. Ý tưởng là buộc mọi phần tử từ chỉ mục$m$trở đi ít nhất là$x$, bởi vì nếu tất cả chúng ít nhất$x$, số trung vị cũng ít nhất là$x$. 
3. Để tính toán chi phí để đạt được điều này, chúng tôi lặp lại từ chỉ mục$m$ĐẾN$n-1$và tích lũy thâm hụt$\max(0, x - a[i])$. Điều này thể hiện số lượng gia tăng tối thiểu cần thiết để nâng các phần tử này lên$x$. Lý do là việc tăng các phần tử bên trái của số trung vị không giúp đảm bảo phạm vi tiếp cận của số trung vị$x$, bởi vì chúng có thể bị đẩy ra khỏi vùng trung vị bởi các phần tử lớn hơn. 
4. Nếu tổng chi phí yêu cầu nằm trong khoảng$k$, thì giá trị$x$là khả thi. Nếu không thì nó quá lớn. 
5. Chúng tôi tìm kiếm nhị phân$x$. Giới hạn dưới là giá trị trung bình hiện tại và giới hạn trên có thể được mở rộng bằng cách cộng tất cả$k$các phép toán đối với phần tử trung vị trong trường hợp tốt nhất. 
6. Đáp án cuối cùng lớn nhất khả thi$x$. 

### Tại sao nó hoạt động 

The crucial invariant is that in a sorted array, the median is determined entirely by the suffix starting at index$m$. Bất kỳ chiến lược nào tạo thành công tất cả các phần tử trong hậu tố này ít nhất$x$đảm bảo số trung vị cũng ít nhất là$x$, bởi vì ít nhất một nửa số phần tử sẽ$\ge x$. Ngược lại, nếu hậu tố không thể nâng lên thành$x$trong phạm vi ngân sách thì bất kỳ nỗ lực nào để đạt được mức trung bình$x$không thành công vì một số phần tử trong vùng kiểm soát sẽ vẫn ở mức dưới$x$, buộc trung vị xuống. Điều này làm cho việc kiểm tra tính khả thi vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(a, m, k, x):
    cost = 0
    for i in range(m, len(a)):
        if a[i] < x:
            cost += x - a[i]
            if cost > k:
                return False
    return True

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    m = n // 2

    lo = a[m]
    hi = a[m] + k

    ans = lo
    while lo <= hi:
        mid = (lo + hi) // 2
        if can(a, m, k, mid):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    print(ans)

if __name__ == "__main__":
    solve()
```Bước sắp xếp là cần thiết vì nó khóa vị trí trung bình để mọi lý do về tính khả thi đều mang tính vị trí thay vì dựa trên giá trị. Chức năng trợ giúp`can`trực tiếp mã hóa ý tưởng rằng chỉ nửa bên phải góp phần thực thi giá trị trung bình ứng viên. 

Tìm kiếm nhị phân ở đây an toàn vì tính khả thi là đơn điệu: nếu một giá trị$x$có thể đạt được thì bất kỳ giá trị nhỏ hơn nào cũng có thể đạt được vì nó không yêu cầu thực hiện thêm thao tác nào. 

Lựa chọn giới hạn trên$a[m] + k$phản ánh kịch bản tốt nhất có thể trong đó tất cả các hoạt động được tập trung vào chính phần tử trung vị. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 3 5
```Mảng được sắp xếp là`[1, 3, 5]`, chỉ số trung vị là`1`. 

Chúng tôi kiểm tra tính khả thi: 

| x | giá từ hậu tố [3,5] | khả thi | 
| --- | --- | --- | 
| 3 | 0 | vâng | 
| 4 | 1 (chỉ 5 thôi là được rồi, 3 cần +1) | vâng | 
| 5 | 2 | vâng | 
| 6 | 3 | không | 

Tìm kiếm nhị phân dừng ở 5. 

Điều này cho thấy cả phần tử ở giữa và phần bên phải phải được nâng lên cùng nhau; chỉ tăng trung vị là không đủ vì hậu tố hạn chế tính khả thi. 

### Ví dụ 2 

đầu vào:```
5 3
1 2 10 10 10
```Mảng được sắp xếp là`[1, 2, 10, 10, 10]`, chỉ số trung vị là`2`. 

| x | giá cho [10,10,10] | tổng chi phí | khả thi | 
| --- | --- | --- | --- | 
| 10 | 0 | 0 | vâng | 
| 11 | 3 | 3 | vâng | 
| 12 | 6 | 6 | không | 

Đáp án là 11. 

Điều này chứng tỏ rằng một khi hậu tố đã chứa các giá trị lớn thì chỉ cần nâng một phần và ngân sách sẽ trực tiếp kiểm soát mức độ có thể đẩy trung vị lên trên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log (k + \max a))$| Sắp xếp mất$O(n \log n)$, mỗi lần kiểm tra tính khả thi là$O(n)$và tìm kiếm nhị phân chạy theo phạm vi logarit trên các giá trị trung bình có thể có | 
| Không gian |$O(1)$thêm | Chỉ có bộ lưu trữ mảng được sử dụng ngoài đầu vào | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử và hệ số logarit tối đa là khoảng 30, làm cho việc này nhanh chóng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()
    m = n // 2

    def can(x):
        cost = 0
        for i in range(m, n):
            if a[i] < x:
                cost += x - a[i]
                if cost > k:
                    return False
        return True

    lo, hi = a[m], a[m] + k
    ans = lo
    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    return str(ans)

# samples
assert run("3 2\n1 3 5\n") == "5"

# custom: minimum
assert run("1 10\n5\n") == "15"

# all equal
assert run("5 3\n7 7 7 7 7\n") == "8"

# large skew
assert run("5 5\n1 1 1 1 10\n") == "3"

# no budget
assert run("3 0\n1 2 3\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | tăng trực tiếp | hành vi trường hợp cơ bản | 
| tất cả đều bình đẳng | nâng đồng phục | xử lý đối xứng | 
| mảng lệch | chi phí theo hậu tố | tính đúng đắn của chi phí tham lam | 
| ngân sách bằng không | không thay đổi | ràng buộc ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi trung vị đã là phần tử lớn nhất trong mảng. Trong trường hợp đó, hậu tố rất nhỏ và chỉ tăng vị trí trung bình có vẻ là đủ, nhưng việc kiểm tra tính khả thi sẽ tính toán chính xác bất kỳ yếu tố nào ở bên phải vẫn cần được nâng lên. 

Một trường hợp khác xảy ra khi$k$là cực kỳ lớn. Tìm kiếm nhị phân sẽ đẩy giá trị trung bình lên trên thậm chí vượt xa tất cả các phần tử hiện có và chi phí hoàn toàn được xác định bởi số lượng phần tử nằm trong hậu tố chứ không phải bởi sự phân phối ban đầu của chúng.
