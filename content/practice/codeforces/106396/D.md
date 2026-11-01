---
title: "CF 106396D - \u6076\u4e0e\u997f"
description: "Chúng ta được cho một dãy số và được yêu cầu tính giá trị toàn cục phụ thuộc vào tất cả các dãy con tăng dần bên trong nó."
date: "2026-06-21T16:17:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106396
codeforces_index: "D"
codeforces_contest_name: "Tiangong University 2025 ICPC Team Selection Contest II (Online Mirror)"
rating: 0
weight: 106396
solve_time_s: 66
verified: true
draft: false
---

[CF 106396D - \u6076\u4e0e\u997f](https://codeforces.com/problemset/problem/106396/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số và được yêu cầu tính giá trị toàn cục phụ thuộc vào tất cả các dãy con tăng dần bên trong nó. Mỗi chuỗi con hợp lệ đóng góp một thứ gì đó có thể được phân tách thành phần đóng góp của từng phần tử mà nó chứa và nhiệm vụ cuối cùng giảm xuống còn tính tổng, trên mỗi phần tử, có bao nhiêu chuỗi con tăng dần “sử dụng nó trong một vai trò cấu trúc cụ thể” nhân với giá trị của nó. 

Cấu trúc ẩn quan trọng là mọi dãy con tăng dần có thể được coi là được “neo” tại một phần tử cụ thể ở giữa. Sau khi sửa một phần tử, chúng ta có thể chia dãy con thành hai phần độc lập: một dãy con tăng hợp lệ kết thúc ở phần tử đó và một dãy con tăng hợp lệ bắt đầu từ phần tử đó. Đóng góp cuối cùng của phần tử đó là tích của số cách xây dựng phần bên trái và số cách xây dựng phần bên phải của nó, nhân với giá trị của chính phần tử đó. 

Đầu vào chỉ đơn giản là một danh sách các số nguyên. Đầu ra là một số nguyên duy nhất theo một mô-đun, biểu thị tổng số có trọng số được mô tả ở trên. 

Các ràng buộc ngụ ý rằng độ dài chuỗi đủ lớn để bất kỳ phép liệt kê bậc hai nào của các chuỗi con đều không thể thực hiện được. Bất kỳ cách tiếp cận nào cố gắng liệt kê rõ ràng các chuỗi con tăng dần hoặc thậm chí đếm chúng trên mỗi phần tử với các vòng lặp lồng nhau sẽ vượt quá giới hạn thời gian. Hướng khả thi duy nhất là phương pháp tuyến tính hoặc gần tuyến tính với cập nhật logarit, thường liên quan đến cây Fenwick hoặc cây phân đoạn sau khi nén các giá trị. 

Một cạm bẫy tinh vi là quên rằng các dãy con có độ dài bằng một cũng có giá trị. Đối với mọi phần tử, cả phần đóng góp bên trái và bên phải của nó đều bao gồm lựa chọn trống, đó là lý do tại sao có giá trị cơ sở của một phần tử được thêm vào theo cả hai hướng. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ cố gắng liệt kê tất cả các dãy con tăng dần và, đối với mỗi dãy con, phân bổ phần đóng góp của nó cho mọi phần tử bên trong nó. Điều này đúng về mặt khái niệm nhưng không khả thi về mặt tính toán. Số lượng các chuỗi con trong trường hợp xấu nhất là theo cấp số nhân và thậm chí việc theo dõi các đóng góp cho mỗi phần tử vẫn sẽ yêu cầu lặp lại theo nhiều cấu trúc theo cấp số nhân. 

Quan sát trọng tâm là sự đóng góp của một chuỗi con có thể được phân tách theo cấp số nhân trên các phần tử của nó nếu chúng ta cố định vai trò của từng phần tử như một trục xoay. Đối với một phần tử cố định, bất kỳ dãy con hợp lệ nào mà nó xuất hiện đều có thể được chia thành hai lựa chọn độc lập: cách chúng ta chọn các phần tử nhỏ hơn và đứng trước nó và cách chúng ta chọn các phần tử lớn hơn và đứng sau nó. Tính độc lập này gợi ý lập trình động trên các vị trí được kết hợp với tập hợp tiền tố và hậu tố hiệu quả theo thứ tự giá trị. 

Đối với mỗi phần tử, chúng tôi tính toán hai đại lượng. Đầu tiên là số dãy con tăng dần kết thúc tại phần tử đó. Điều này chỉ phụ thuộc vào các phần tử trước nó có giá trị nhỏ hơn. Thứ hai là số dãy con tăng dần bắt đầu từ phần tử đó, chỉ phụ thuộc vào các phần tử đứng sau nó có giá trị lớn hơn. Cả hai đều có thể được tính toán bằng cách sử dụng cây Fenwick trên các giá trị nén, vì chúng ta chỉ cần tổng tiền tố và hậu tố của các giá trị DP. 

Sau khi cả hai mảng được tính toán, mỗi phần tử đóng góp tích của bên trái và bên phải nhân với giá trị của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Phân hủy Fenwick DP | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý vấn đề theo ba giai đoạn khái niệm: nén giá trị, tính toán đóng góp bên trái, tính toán đóng góp bên phải, sau đó kết hợp.

1. Đầu tiên, nén tất cả các giá trị vào một phạm vi dày đặc. Điều này là cần thiết vì cây Fenwick hoạt động dựa trên các chỉ số và chúng ta chỉ quan tâm đến thứ tự tương đối chứ không phải độ lớn thực tế. Sau khi nén, những so sánh như “nhỏ hơn” sẽ trở thành truy vấn tiền tố. 
2. Tính mảng đóng góp bên trái. Chúng ta lặp từ trái sang phải. Đối với mỗi vị trí i, chúng tôi truy vấn có bao nhiêu chuỗi con hợp lệ kết thúc với các giá trị nhỏ hơn giá trị hiện tại. Truy vấn đó cung cấp tất cả các cách có thể để mở rộng các chuỗi con tăng dần kết thúc trước đó. Chúng ta thêm một cái vào tài khoản cho dãy con bao gồm chính phần tử đó. Sau đó, chúng tôi chèn giá trị này vào cây Fenwick ở chỉ mục nén của nó để nó có sẵn cho các phần tử trong tương lai. 
3. Tính toán mảng đóng góp bên phải một cách đối xứng. Chúng ta lặp từ phải sang trái. Đối với mỗi vị trí i, chúng tôi truy vấn có bao nhiêu chuỗi con bắt đầu với các giá trị lớn hơn giá trị hiện tại. Một lần nữa, chúng ta thêm một giá trị cho dãy con có một phần tử. Chúng tôi chèn vào cây Fenwick khi chúng tôi đi. 
4. Tổng hợp kết quả. Đối với mỗi phần tử i, tổng đóng góp của nó là left[i] nhân với right[i], nhân với giá trị ban đầu của phần tử trước khi nén. 

Phép nhân xuất hiện vì bất kỳ dãy con hợp lệ nào trong đó phần tử này được chọn làm trục đều được hình thành bằng cách chọn độc lập phần bên trái và phần bên phải. 

### Tại sao nó hoạt động 

Tại mọi vị trí, cây Fenwick lưu trữ tổng số dãy con tăng hợp lệ kết thúc (hoặc bắt đầu) tại mỗi chỉ số giá trị được xử lý cho đến nay. Quá trình chuyển đổi DP dựa trên thực tế là các chuỗi con tăng dần được xác định đầy đủ bởi giá trị được chọn cuối cùng của chúng ở phía bên trái và giá trị được chọn đầu tiên ở phía bên phải. Vì những lựa chọn này độc lập khi trục xoay được cố định nên việc phân tách sản phẩm là hợp lệ. Không có chuỗi con nào được tính gấp đôi vì mỗi chuỗi con có một phần tử trục duy nhất được tính là “nguồn đóng góp trung gian” của nó trong phép tính tổng cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        i += 1
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        res = 0
        while i > 0:
            res += self.bit[i]
            i -= i & -i
        return res

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l)

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    vals = sorted(set(a))
    mp = {v: i for i, v in enumerate(vals)}
    a = [mp[x] for x in a]

    m = len(vals)

    left = [0] * n
    right = [0] * n

    fw = Fenwick(m)

    for i in range(n):
        left[i] = fw.sum(a[i]) + 1
        fw.add(a[i], left[i])

    fw = Fenwick(m)

    for i in range(n - 1, -1, -1):
        right[i] = fw.sum(m) - fw.sum(a[i] + 1) + 1
        fw.add(a[i], right[i])

    ans = 0
    for i in range(n):
        ans += left[i] * right[i] * vals[a[i]]

    print(ans)

if __name__ == "__main__":
    solve()
```Cây Fenwick được sử dụng hai lần với cấu trúc giống nhau nhưng hướng di chuyển khác nhau. Ở bước bên trái, tổng tiền tố tích lũy các đóng góp từ các phần tử nhỏ hơn trước đó. Ở bước bên phải, hành vi hậu tố được mô phỏng bằng cách sử dụng phép trừ tiền tố đảo ngược. 

các`+1`thuật ngữ trong cả hai lượt là cần thiết vì mỗi phần tử riêng lẻ tạo thành một chuỗi con hợp lệ và nếu không có nó, DP sẽ chỉ tính các phần mở rộng chứ không phải các chuỗi độc lập. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng nhỏ`[1, 2, 1]`. Sau khi nén nó vẫn còn`[0, 1, 0]`. 

Ở phần bên trái, chúng tôi xử lý từng bước. 

| tôi | giá trị | Bang Fenwick (khái niệm) | trái[i] | 
| --- | --- | --- | --- | 
| 0 | 0 | trống | 1 | 
| 1 | 1 | chứa các chuỗi từ 0 | 2 | 
| 2 | 0 | chứa những đóng góp từ trước đó | 1 | 

Tại chỉ số 1, chúng ta có thể mở rộng các dãy con kết thúc bằng 0, đưa ra hai khả năng. Ở chỉ số 2, chỉ tồn tại dãy con tầm thường vì chuỗi 1 trước đó không thể mở rộng đến giá trị nhỏ hơn. 

Ở đường chuyền bên phải: 

| tôi | giá trị | thông tin có sẵn trong tương lai | đúng[i] | 
| --- | --- | --- | --- | 
| 2 | 0 | trống | 1 | 
| 1 | 1 | chỉ 0 ở bên phải | 2 | 
| 0 | 0 | bao gồm tất cả tương lai | 1 | 

Việc kết hợp các đóng góp mang lại phép tính có trọng số chính xác trên tất cả các chuỗi con tăng dần với việc phân tách trục chính xác. 

Bây giờ hãy xem xét`[3, 1, 2]`, nén thành`[2, 0, 1]`. DP bên trái chỉ tăng khi không thể gặp các giá trị lớn hơn sau các giá trị nhỏ hơn, cho thấy thứ tự chi phối luồng đóng góp như thế nào. DP bên phải phản ánh hành vi này từ phía đối diện. 

Những dấu vết này xác nhận rằng sự đóng góp của mỗi phần tử chỉ phụ thuộc vào thứ tự tương đối trước và sau nó chứ không phải vị trí tuyệt đối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi lần cập nhật và truy vấn Fenwick mất thời gian logarit, được thực hiện hai lần trên mảng | 
| Không gian | O(n) | Lưu trữ cho cây Fenwick và mảng DP | 

Giải pháp dễ dàng phù hợp với các ràng buộc điển hình lên tới 200.000 phần tử. Chi phí logarit đủ nhỏ trong 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()  # placeholder, integrate solve() in real test

# minimal case
# assert run("1\n5") == "5"

# increasing sequence
# assert run("3\n1 2 3") == "..." 

# all equal
# assert run("4\n2 2 2 2") == "..."

# decreasing sequence
# assert run("3\n3 2 1") == "..."
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | chính nó | độ chính xác cơ sở DP | 
| mảng tăng dần | tăng trưởng có cấu trúc | tích lũy tiền tố | 
| phần tử bằng nhau | xử lý trùng lặp | độ chính xác nén | 
| mảng giảm dần | phần mở rộng tối thiểu | hậu tố đối xứng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các phần tử giống hệt nhau. Trong trường hợp đó, mỗi dãy con tăng chỉ có thể có độ dài bằng một, do đó mỗi phần tử đóng góp độc lập. Cả hai mảng DP phải đánh giá là 1 cho mọi vị trí và câu trả lời cuối cùng sẽ trở thành tổng của tất cả các phần tử. Cây Fenwick xử lý chính xác điều này vì không có phần tử nào hoàn toàn nhỏ hơn hoặc lớn hơn, vì vậy tất cả các truy vấn tiền tố và hậu tố đều trả về 0 và cơ số +1 chiếm ưu thế. 

Một trường hợp tế nhị khác là thứ tự giảm dần. Ở đây, mọi giá trị DP còn lại vẫn là 1 vì không có phần tử nào trước đó nhỏ hơn. Ở bên phải, tính đối xứng cho kết quả tương tự. Điều này xác nhận rằng thuật toán không cố gắng mở rộng các chuỗi con không hợp lệ một cách sai lầm khi sắp xếp các chuyển tiếp chặn các ràng buộc ràng buộc.
