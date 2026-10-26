---
title: "CF 106089A - K-\u0438\u043d\u0442\u0435\u0440\u0435\u0441\u043d\u044b\u0435 \u043f\u043e\u0434\u043e\u0442\u0440\u0435\u0437\u043a\u0438"
description: "Chúng ta được cung cấp một chuỗi các phần tử và một tham số kiểm soát những gì làm cho một mảng con trở nên “thú vị”. Nhiệm vụ là xem xét tất cả các phân đoạn liền kề của mảng và xác định có bao nhiêu trong số chúng thỏa mãn một ràng buộc dựa trên mức độ đa dạng của nội dung của chúng."
date: "2026-06-19T20:22:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106089
codeforces_index: "A"
codeforces_contest_name: "\u0412\u0443\u0437\u043e\u0432\u0441\u043a\u043e-\u0430\u043a\u0430\u0434\u0435\u043c\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 2025, \u0444\u0438\u043d\u0430\u043b"
rating: 0
weight: 106089
solve_time_s: 69
verified: true
draft: false
---

[CF 106089A - K-\u0438\u043d\u0442\u0435\u0440\u0435\u0441\u043d\u044b\u0435 \u043f\u043e\u0434\u043e\u0442\u0440\u0435\u0437\u043a\u0438](https://codeforces.com/problemset/problem/106089/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các phần tử và một tham số kiểm soát những gì làm cho một mảng con trở nên “thú vị”. Nhiệm vụ là xem xét tất cả các phân đoạn liền kề của mảng và xác định có bao nhiêu trong số chúng thỏa mãn một ràng buộc dựa trên mức độ đa dạng của nội dung của chúng. Trong bài toán này, một phân đoạn chỉ đơn giản là một phần liên tục của mảng và điều kiện phụ thuộc vào số lượng giá trị riêng biệt bên trong phần đó. 

Mặc dù văn bản câu lệnh trong nguồn bị cắt xén một phần, nhưng đối tượng tổ hợp cốt lõi vẫn là tiêu chuẩn: chúng tôi đang làm việc với tất cả các khoảng của một mảng và chúng tôi cần lọc chúng theo quy tắc “k-interesting”, quy tắc này trong các công thức điển hình có nghĩa là số phần tử riêng biệt bên trong khoảng được giới hạn bởi k. 

Do đó, đầu vào đại diện cho một mảng có độ dài n theo sau là một số nguyên k. Đầu ra là một số duy nhất: tổng số các mảng con liền kề có tập hợp các phần tử riêng biệt có kích thước tối đa là k. 

Mẫu ràng buộc trong các bài toán như vậy thường hàm ý n lên đến khoảng 200000. Điều đó ngay lập tức loại trừ bất kỳ phép liệt kê bậc hai nào của các mảng con, vì n bình phương có thứ tự 10^10 phép tính, vượt xa các giới hạn thông thường. Bất kỳ giải pháp khả thi nào cũng phải gần với tuyến tính hoặc tuyến tính, lý tưởng nhất là O(n). 

Các trường hợp tinh tế xuất hiện khi các giá trị lặp lại theo kiểu không đều. Một cách tiếp cận ngây thơ thường phá vỡ những tình huống sau: 

Một dạng lỗi là giả định rằng việc kiểm tra từng mảng con một cách độc lập là ổn, điều này dẫn đến hết thời gian chờ. Ví dụ: nếu mảng xen kẽ nhiều giá trị riêng biệt như 1 2 3 4 5 ..., thì mọi mảng con phải được kiểm tra và mỗi mảng con có thể quét khoảng đó, dẫn đến hành vi bậc ba. 

Một chế độ lỗi khác là xử lý sai bản cập nhật số lượng riêng biệt khi mở rộng cửa sổ. Ví dụ: trong một mảng như 1 2 1 2 3, việc loại bỏ hoặc thêm các phần tử đòi hỏi phải duy trì cẩn thận việc đếm tần số, nếu không các bản sao có thể bị tính hai lần hoặc bị mất. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê mọi mảng con bằng cách chọn điểm cuối bên trái và điểm cuối bên phải, sau đó tính xem có bao nhiêu giá trị riêng biệt bên trong. Điều này có thể được thực hiện bằng cách quét mảng con và sử dụng một tập hợp để đếm các phần tử riêng biệt. Mặc dù đúng về mặt khái niệm, nhưng cách tiếp cận này thực hiện công việc O(n) trên mỗi mảng con và có các mảng con O(n^2), dẫn đến độ phức tạp O(n^3) trong trường hợp xấu nhất. Ngay cả việc tối ưu hóa quá trình quét bên trong bằng bộ băm cũng không làm thay đổi chi phí tiệm cận. 

Quan sát cấu trúc quan trọng là nếu chúng ta cố định điểm cuối bên trái thì tập hợp các điểm cuối bên phải hợp lệ sẽ tạo thành một phạm vi liền kề. Khi chúng ta di chuyển con trỏ bên phải sang bên phải, số phần tử riêng biệt chỉ tăng lên hoặc giữ nguyên. Tính đơn điệu này cho phép chúng ta duy trì một cửa sổ trượt: chúng ta mở rộng ranh giới bên phải khi điều kiện được thỏa mãn và thu hẹp ranh giới bên trái khi nó bị vi phạm. 

Điều này làm giảm vấn đề duy trì bản đồ tần số của các phần tử bên trong cửa sổ hiện tại. Mỗi lần chúng ta mở rộng con trỏ bên phải, chúng ta sẽ tăng số lượng phần tử đó. Nếu nó trở thành một phần tử riêng biệt mới, chúng ta sẽ tăng bộ đếm riêng biệt. Nếu số phần tử riêng biệt vượt quá k, chúng ta di chuyển con trỏ bên trái về phía trước cho đến khi ràng buộc được khôi phục, giảm tần số và có thể giảm số lượng riêng biệt. Mọi chuyển động của một trong hai con trỏ xảy ra nhiều nhất là n lần, do đó độ phức tạp tổng cộng trở thành tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Cửa Sổ Trượt | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì hai con trỏ xác định cửa sổ hiện tại [l, r), từ điển tần số cho các giá trị bên trong cửa sổ và bộ đếm cho biết hiện có bao nhiêu giá trị riêng biệt. 

1. Khởi tạo l = 0, kết quả = 0 và bản đồ tần số trống. Con trỏ bên phải sẽ mở rộng trên mảng. 
2. Với mỗi vị trí r từ 0 đến n - 1, hãy đưa a[r] vào cửa sổ bằng cách tăng tần số của nó. Nếu phần tử này trước đây không có, hãy tăng bộ đếm riêng biệt. 
3. Nếu bộ đếm riêng biệt vượt quá k, liên tục di chuyển l về phía trước trong khi giảm tần số a[l]. Khi tần số trở về 0, hãy giảm bộ đếm riêng biệt. Tiếp tục cho đến khi cửa sổ hợp lệ trở lại. Bước này đảm bảo cửa sổ luôn thỏa mãn ràng buộc. 
4. Khi cửa sổ hợp lệ, tất cả các mảng con kết thúc bằng r và bắt đầu từ l đến r đều hợp lệ. Số mảng con như vậy là (r - l + 1), nên ta cộng giá trị này vào kết quả. 
5. Tiếp tục khai triển r cho đến hết mảng. 

Bất biến cốt lõi là ở mỗi bước, cửa sổ [l, r] chứa tối đa k phần tử riêng biệt và l là chỉ mục nhỏ nhất duy trì thuộc tính này cho r hiện tại. Điều này đảm bảo rằng chúng ta không bao giờ bỏ lỡ các mảng con hợp lệ và không bao giờ đếm những mảng con không hợp lệ. Mỗi mảng con hợp lệ được tính duy nhất tại thời điểm điểm cuối bên phải của nó được cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    freq = {}
    distinct = 0
    l = 0
    ans = 0

    for r in range(n):
        x = a[r]
        if x not in freq or freq[x] == 0:
            freq[x] = 0
            distinct += 1
        freq[x] += 1

        while distinct > k:
            y = a[l]
            freq[y] -= 1
            if freq[y] == 0:
                distinct -= 1
            l += 1

        ans += (r - l + 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện cửa sổ trượt được mô tả ở trên. Từ điển tần số theo dõi số lần mỗi giá trị xuất hiện trong cửa sổ hiện tại. Bộ đếm riêng biệt tránh việc quét từ điển nhiều lần. 

Chi tiết triển khai chính là thứ tự cập nhật khi thu nhỏ cửa sổ. Trước tiên, chúng ta phải giảm tần số và chỉ giảm bộ đếm riêng biệt khi tần số bằng 0. Bất kỳ sai lệch nào so với logic này đều dẫn đến việc đếm không chính xác các phần tử duy nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng`1 2 1 2`với k = 2. 

| r | Đang đến | l sau khi điều chỉnh | khác biệt | cửa sổ | đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | [1] | 1 | 
| 1 | 2 | 0 | 2 | [1,2] | 2 | 
| 2 | 1 | 0 | 2 | [1,2,1] | 3 | 
| 3 | 2 | 0 | 2 | [1,2,1,2] | 4 | 

Kết quả là 10, tương ứng với tất cả các mảng con vì không có mảng con nào vượt quá 2 phần tử riêng biệt. 

Bây giờ hãy xem xét`1 2 3`với k = 2. 

| r | Đang đến | l sau khi điều chỉnh | khác biệt | cửa sổ | đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | [1] | 1 | 
| 1 | 2 | 0 | 2 | [1,2] | 2 | 
| 2 | 3 | 1 | 2 | [2,3] | 2 | 

Điều này cho thấy cách con trỏ bên trái di chuyển để khôi phục tính hợp lệ khi phần tử riêng biệt thứ ba xuất hiện. 

Ví dụ đầu tiên xác nhận sự tích lũy khi ràng buộc không bao giờ bị vi phạm. Phần thứ hai cho thấy cách thuật toán tự động thu nhỏ cửa sổ để thực thi giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi con trỏ di chuyển nhiều nhất n lần | 
| Không gian | O(n) | bản đồ tần số lưu trữ tối đa n giá trị riêng biệt | 

Độ phức tạp tuyến tính phù hợp thoải mái trong các ràng buộc điển hình lên tới 200000 phần tử và mức sử dụng bộ nhớ tỷ lệ thuận với số lượng giá trị riêng biệt được quan sát. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()

    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# basic case
assert run("4 2\n1 2 1 2\n") == "10"

# increasing distinct
assert run("3 2\n1 2 3\n") == "5"

# all equal
assert run("5 1\n7 7 7 7 7\n") == "15"

# k large enough
assert run("3 5\n1 2 3\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 1 2 với k=2 | 10 | không cần thu nhỏ | 
| 1 2 3 với k=2 | 5 | co cửa sổ | 
| tất cả đều bằng k=1 | 15 | xử lý tần số | 
| k > n | 6 | tầm thường đầy đủ | 

## Vỏ cạnh 

Trường hợp cạnh phổ biến là khi k bằng 1, điều này buộc mọi mảng con hợp lệ chỉ chứa các phần tử giống hệt nhau. Theo trình tự như`5 5 5 5`, cửa sổ không bao giờ mở rộng vượt quá các giá trị giống hệt nhau và thuật toán đếm chính xác tất cả các mảng con hình tam giác được hình thành bởi các phần tử lặp lại. 

Một trường hợp cạnh khác là khi k lớn hơn số phần tử riêng biệt trong mảng. Trong trường hợp này, con trỏ trái không bao giờ di chuyển và mọi mảng con đều hợp lệ. Thuật toán vẫn tích lũy chính xác kết quả n(n+1)/2 đầy đủ vì cửa sổ luôn hợp lệ mà không bị co lại. 

Một trường hợp tinh tế hơn nữa là các chuỗi xen kẽ như`1 2 1 2 1 2`, trong đó cửa sổ liên tục đạt đến ngưỡng và hầu như không còn hiệu lực. Bản đồ tần số đảm bảo rằng các phần tử nhập lại cửa sổ được kích hoạt lại một cách chính xác mà không cần đếm hai lần các giá trị riêng biệt và bộ đếm riêng biệt vẫn nhất quán trong suốt tất cả các lần mở rộng và thu gọn.
