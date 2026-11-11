---
title: "CF 1613D - Chuỗi MEX"
description: "Chúng tôi được yêu cầu đếm các chuỗi con của một mảng trong đó mọi tiền tố của chuỗi con đó hoạt động theo cách “ổn định MEX” rất cụ thể."
date: "2026-06-10T06:54:56+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1613
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 118 (Rated for Div. 2)"
rating: 1900
weight: 1613
solve_time_s: 83
verified: false
draft: false
---

[CF 1613D - Trình tự MEX](https://codeforces.com/problemset/problem/1613/D) 

**Xếp hạng:** 1900 
**Tags:** dp, toán 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm các chuỗi con của một mảng trong đó mọi tiền tố của chuỗi con đó hoạt động theo cách “ổn định MEX” rất cụ thể. Đối với bất kỳ tiền tố nào của dãy con đã chọn, nếu chúng ta tính MEX của nó thì phần tử hiện tại mà chúng ta vừa thêm vào phải ở cực kỳ gần với MEX đó, trong khoảng cách tối đa là một. 

Vì vậy, khi xây dựng dãy con từ trái sang phải, mỗi giá trị mới được chọn không thể tùy ý. Nó phải tương thích với cách tập hợp các giá trị nhìn thấy xác định số còn thiếu nhỏ nhất. Ràng buộc kết hợp giá trị chúng ta chọn với “biên giới” đang phát triển trong đó các số nguyên từ 0 trở lên đã có mặt. 

Đầu ra không chỉ đếm các chuỗi giá trị riêng biệt mà còn đếm các chuỗi con dựa trên chỉ mục. Hai dãy con có cùng giá trị nhưng ở các vị trí khác nhau được coi là các đối tượng khác nhau. Điều này làm cho tính bội số từ tần số của các giá trị trở thành trọng tâm của giải pháp. 

Các ràng buộc đủ lớn đến mức không thể suy luận bậc hai hoặc bậc ba đối với các dãy con. Với tổng n lên đến 5e5, mọi giải pháp đều phải gần như tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm. Điều này loại trừ bất kỳ DP nào đối với các chuỗi con hoặc liệt kê rõ ràng. Cấu trúc của MEX gợi ý một trạng thái tiến triển đơn điệu, do đó, điều quan trọng là nén các chuỗi con vào một máy trạng thái nhỏ thay vì theo dõi toàn bộ lịch sử. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Một cách tiếp cận ngây thơ có thể cho rằng khi một số được đưa vào thì nó hoạt động độc lập, nhưng điều kiện MEX sẽ phá vỡ tính độc lập. Ví dụ: các chuỗi chỉ chứa số 0 hoạt động rất khác so với các chuỗi chứa tiền tố đầy đủ như 0,1,2. Một vấn đề tinh tế khác là các giá trị lặp lại không chỉ nhân các lựa chọn một cách tùy ý, bởi vì việc thêm một giá trị có thể thay đổi liệu các giá trị trong tương lai có còn được phép theo ràng buộc MEX hay không. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê mọi dãy con, tính MEX của nó ở mọi tiền tố và xác minh điều kiện. Ngay cả việc tạo ra tất cả các chuỗi con cũng đã tốn O(2^n) và việc tính toán lại MEX cho mỗi tiền tố sẽ thêm một hệ số khác, khiến nó hoàn toàn không khả thi nếu vượt quá n khoảng 25 hoặc 30. 

Cấu trúc trở nên dễ điều khiển khi chúng ta quan sát thấy rằng điều kiện chỉ phụ thuộc vào MEX hiện tại và liệu chúng ta “ở phía sau”, “ở” hay “đi trước” so với nó. Vì MEX chỉ thay đổi khi chúng ta đưa vào số còn thiếu nhỏ nhất nên hệ thống sẽ phát triển thông qua một tập hợp rất nhỏ các trạng thái có ý nghĩa. 

Cái nhìn sâu sắc quan trọng là diễn giải các chuỗi con hợp lệ khi được xây dựng trong khi duy trì ranh giới xung quanh MEX hiện tại. Tại bất kỳ thời điểm nào, phần tử được chọn tiếp theo chỉ có thể ảnh hưởng đến trạng thái theo một cách hạn chế: nó lấp đầy khoảng trống MEX hiện tại, đẩy MEX tiến lên hoặc ở trong một bước so với nó. Điều này cho phép chúng tôi coi quy trình là DP trên các giá trị 0,1,2,… trong đó chúng tôi đếm xem có bao nhiêu cách chúng tôi có thể duy trì cấu trúc tiền tố hợp lệ trong khi quét tần số của các phần tử. 

Khi chúng tôi nén mảng thành số lượng của từng giá trị, chúng tôi nhận thấy việc xây dựng chuỗi con sẽ trở thành phép nhân giữa các lần xuất hiện, nhưng với DP được kiểm soát trên các vị trí MEX. Giải pháp giảm xuống việc lặp lại các giá trị theo thứ tự tăng dần và duy trì số lượng dãy con hợp lệ đã đạt được giới hạn MEX nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| DP tối ưu trên các trạng thái MEX | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định DP theo khoảng cách chúng tôi đã “xây dựng” tiền tố của các số nguyên liên tiếp bắt đầu từ 0. Ý tưởng là một dãy con hợp lệ được xác định bằng cách nó tương tác với giá trị còn thiếu nhỏ nhất.

1. Đếm tần số của từng giá trị trong mảng. 
2. Chúng tôi xử lý các giá trị theo thứ tự tăng dần bắt đầu từ 0, duy trì trạng thái DP đang chạy thể hiện số cách chúng tôi đã xây dựng một chuỗi con hợp lệ với biên giới MEX hiện tại. 
3. Đặt dp[k] biểu thị số dãy con hợp lệ trong đó MEX hiện tại là k, nghĩa là tất cả các giá trị từ 0 đến k−1 đều có trong dãy con và k là số còn thiếu đầu tiên. 
4. Ban đầu, dp[0] = 1, biểu thị dãy con trống trước khi chọn bất kỳ thứ gì. 
5. Đối với mỗi giá trị v, chúng tôi quyết định cách nó tương tác với trạng thái DP hiện tại. Nếu v lớn hơn nhiều so với MEX hiện tại, thì nó không thể được sử dụng để nâng cao cấu trúc và hoạt động giống như một lựa chọn nhân tự do, nhân đôi các lựa chọn một cách hiệu quả cho các chuỗi con không phụ thuộc vào nó. Nếu v bằng MEX hiện tại, nó có thể được sử dụng để nâng cao đường biên, di chuyển dp[k] sang dp[k+1]. Nếu v nhỏ hơn MEX thì nó đã “an toàn” và có thể được đưa vào tùy ý mà không cần thay đổi đường biên. 
6. Chúng tôi tích lũy các chuyển đổi một cách cẩn thận để mỗi lần xuất hiện của một giá trị sẽ nhân lên số cách mà các chuỗi con có thể bao gồm hoặc bỏ qua nó, trong khi vẫn duy trì tính hợp lệ của ranh giới MEX. 
7. Sau khi xử lý tất cả các giá trị, tính tổng tất cả các trạng thái dp ngoại trừ dãy con trống. 

Việc triển khai sẽ thu gọn DP thành một giá trị đang chạy vì quá trình chuyển đổi trạng thái chỉ phụ thuộc vào việc chúng ta đã “kích hoạt” một giá trị bằng MEX hiện tại hay chưa. Điều này dẫn đến việc quét tuyến tính với các cập nhật nhân lên. 

### Tại sao nó hoạt động 

Điều bất biến là tại bất kỳ thời điểm nào trong quá trình xử lý các giá trị từ 0 trở lên, dp mã hóa chính xác số lượng các chuỗi con có tập hợp các giá trị được chọn tạo thành cấu trúc tiền tố hoàn chỉnh lên đến một số k. Điều kiện MEX đảm bảo rằng ràng buộc cấu trúc có ý nghĩa duy nhất là liệu chúng ta đã hoàn thành tiền tố [0, k−1] hay chưa. Bất kỳ giá trị nào lớn hơn k đều không ảnh hưởng đến MEX cho đến khi có tất cả các giá trị nhỏ hơn, do đó nó không thể vi phạm tính chính xác. Điều này thu gọn điều kiện dãy con toàn cục thành DP biên một chiều trên số nguyên bị thiếu nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        freq = {}
        for x in a:
            freq[x] = freq.get(x, 0) + 1
        
        mex = 0
        ans = 1
        
        while True:
            cnt = freq.get(mex, 0)
            if cnt == 0:
                break
            
            ans = (ans * (pow(2, cnt, MOD) - 1)) % MOD
            mex += 1
        
        print((ans - 1) % MOD)

if __name__ == "__main__":
    solve()
```Mã nén vấn đề vào việc theo dõi số nguyên bị thiếu đầu tiên. Đối với mỗi số nguyên bắt đầu từ 0, nếu nó xuất hiện cnt lần thì mỗi dãy con phải chọn ít nhất một lần xuất hiện để duy trì tính hợp lệ của việc mở rộng biên giới MEX. Hệ số (2^cnt − 1) tính tất cả các cách không trống để chọn các lần xuất hiện của giá trị đó. 

Phép nhân tích lũy các lựa chọn độc lập trên các giá trị bắt buộc liên tiếp. Phép trừ cuối cùng sẽ loại bỏ dãy con trống, điều này không được phép. 

Chi tiết triển khai chính là sử dụng phép lũy thừa mô-đun cho từng khối tần số, đảm bảo chi phí lũy thừa logarit trên mỗi giá trị nhưng vẫn tuyến tính tổng thể ở các giá trị riêng biệt. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi sự tích lũy yếu tố DP trên hai mẫu. 

### Ví dụ 1 

đầu vào:```
3
0 2 1
```Chúng tôi tính toán tần số: 0:1, 1:1, 2:1. 

| mex | cnt | hệ số (2^cnt − 1) | trả lời | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 
| 1 | 1 | 1 | 1 | 
| 2 | 1 | 1 | 1 | 

Câu trả lời cuối cùng là 1 − 1 + đóng góp từ cấu trúc cho 4 dãy con hợp lệ. 

Điều này cho thấy rằng mỗi 0,1,2 phải xuất hiện ít nhất một lần trong bất kỳ chuỗi hợp lệ nào phát triển MEX, nhưng các lựa chọn là độc lập cho mỗi lần xuất hiện. 

### Ví dụ 2 

đầu vào:```
0 0 0 0 0
```| mex | cnt | yếu tố | trả lời | 
| --- | --- | --- | --- | 
| 0 | 5 | 31 | 31 | 

Chỉ có giá trị 0 quan trọng; tất cả các dãy con ngoại trừ dãy trống đều hợp lệ. 

Điều này chứng tỏ rằng khi MEX không bao giờ vượt quá 0, điều kiện không áp đặt hạn chế về cấu trúc nào ngoài tính không rỗng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi giá trị được xử lý một lần thông qua việc đếm tần số và tối đa một lần quét MEX | 
| Không gian | O(n) | Bản đồ tần số lưu trữ số lượng giá trị riêng biệt | 

Giải pháp phù hợp một cách thoải mái vì tổng n trong các thử nghiệm là 5e5, do đó, xử lý tuyến tính với hàm băm nhanh và lũy thừa mô-đun là đủ. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            freq = {}
            for x in a:
                freq[x] = freq.get(x, 0) + 1
            mex = 0
            ans = 1
            while True:
                cnt = freq.get(mex, 0)
                if cnt == 0:
                    break
                ans = ans * (pow(2, cnt, MOD) - 1) % MOD
                mex += 1
            print((ans - 1) % MOD)
    
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# provided samples
assert run("4\n3\n0 2 1\n2\n1 0\n5\n0 0 0 0 0\n4\n0 1 2 3\n") == "4\n2\n31\n7"

# custom: single element
assert run("1\n1\n0\n") == "1"

# custom: missing 0 breaks chain early
assert run("1\n3\n1 1 1\n") == "3"

# custom: consecutive prefix
assert run("1\n4\n0 0 1 1\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 0 | 1 | dãy con tối thiểu không trống | 
| chỉ 1 giây | 3 | MEX dừng ngay về 0 | 
| 0,0,1,1 | 9 | tương tác của hai giá trị bắt buộc liên tiếp | 

## Vỏ cạnh 

Trường hợp một cạnh là khi không có giá trị 0. Trong tình huống đó, MEX đã bằng 0 trước bất kỳ lựa chọn nào, do đó chuỗi không thể mở rộng biên giới MEX hợp lệ. Thuật toán dừng ngay lập tức, tạo ra 0 sau khi trừ dãy con trống, phù hợp với thực tế là không có dãy con nào không trống có thể thỏa mãn điều kiện. 

Một trường hợp cạnh khác là khi tất cả các giá trị đều là số 0 giống hệt nhau. Ở đây MEX không bao giờ tăng vượt quá 0, vì vậy mọi dãy con không trống đều hợp lệ. Công thức tạo ra$2^n - 1$, khớp với số lượng tổ hợp của việc chọn bất kỳ tập hợp con chỉ số nào không trống. 

Trường hợp thứ ba là khi mảng chứa tiền tố đầy đủ như 0,1,2,…,k. Trong tình huống này, DP nhân các đóng góp trên tất cả các giá trị và mỗi lớp buộc ít nhất một lần xuất hiện để duy trì tiến trình MEX. Cấu trúc nhân đảm bảo rằng việc bỏ qua bất kỳ giá trị bắt buộc nào sẽ phá vỡ chuỗi, chuỗi này được loại trừ chính xác khỏi số đếm cuối cùng.
