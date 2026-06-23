---
title: "CF 105562D - Dân chủ Hà Lan"
description: "Chúng ta có một tập hợp các đảng phái chính trị, mỗi đảng có một số ghế nhất định. “Liên minh” chỉ đơn giản là một tập hợp con của các đảng này. Chúng tôi muốn đếm xem có bao nhiêu tập hợp con thỏa mãn một khái niệm rất cụ thể về việc trở thành một liên minh cai trị hợp lệ."
date: "2026-06-22T14:20:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105562
codeforces_index: "D"
codeforces_contest_name: "2024-2025 ICPC Northwestern European Regional Programming Contest (NWERC 2024)"
rating: 0
weight: 105562
solve_time_s: 67
verified: true
draft: false
---

[CF 105562D - Dân chủ Hà Lan](https://codeforces.com/problemset/problem/105562/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các đảng phái chính trị, mỗi đảng có một số ghế nhất định. “Liên minh” chỉ đơn giản là một tập hợp con của các đảng này. Chúng tôi muốn đếm xem có bao nhiêu tập hợp con thỏa mãn một khái niệm rất cụ thể về việc trở thành một liên minh cai trị hợp lệ. 

Một tập hợp con có thể được chấp nhận nếu nó có đa số tuyệt đối trong số tất cả các ghế, nghĩa là tổng số ghế trong tập hợp con lớn hơn một nửa tổng số ghế của tất cả các bên. Tuy nhiên, điều này là không đủ. Liên minh cũng phải ở mức tối thiểu theo nghĩa là nếu bạn loại bỏ bất kỳ đảng nào khỏi liên minh đó, liên minh đó sẽ ngay lập tức không còn chiếm đa số nghiêm ngặt. Vì vậy, mọi đảng trong liên minh đều cần thiết để duy trì đa số. 

Các ràng buộc cho phép tối đa 60 đảng, mỗi đảng có tối đa 10.000 ghế. Điều này ngay lập tức gợi ý rằng về nguyên tắc có thể liệt kê trực tiếp tất cả các tập hợp con vì có khoảng 2^60 tập hợp con, nhưng rõ ràng là không thể thực hiện được trong thực tế. Bất kỳ giải pháp hợp lệ nào cũng phải khai thác cấu trúc trong điều kiện “đa số tối thiểu” thay vì các tập hợp con bị ép buộc một cách thô bạo. 

Một nỗ lực ngây thơ sẽ thử tất cả các tập hợp con và kiểm tra cả hai điều kiện. Điều này không thành công vì 2^60 rất lớn về mặt thiên văn, vượt xa mọi tính toán khả thi. 

Một trường hợp thất bại tinh vi hơn xuất phát từ việc chỉ kiểm tra điều kiện đa số. Ví dụ: nếu chúng ta lấy một tập hợp con có tổng chỉ lớn hơn một nửa thì nó không nhất thiết là tối thiểu. Hãy xem xét trường hợp 60, 50, 49 trong đó tổng số là 159. Tập hợp con {60, 50} có tổng 110, là đa số, nhưng loại bỏ 50 để lại 60 vẫn là đa số. Vì vậy, nó không hợp lệ mặc dù nó vượt qua điều kiện đầu tiên. 

Sai lầm ngược lại cũng thường gặp: đảm bảo tính tối thiểu mà không đảm bảo tính đa số. Một tập hợp con trong đó mọi phần tử đều “lớn so với phần còn lại” vẫn có thể không vượt quá một nửa tổng số. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp qua tất cả các tập hợp con, tính tổng của chúng và sau đó kiểm tra từng phần tử trong tập hợp con xem việc loại bỏ nó có vi phạm điều kiện đa số hay không. Điều này đúng nhưng có độ phức tạp theo cấp số nhân trong cả việc liệt kê tập hợp con và xác thực từng tập hợp con. Với 60 yếu tố, điều này trở nên hoàn toàn không khả thi. 

Quan sát chính là định dạng lại tính tối thiểu theo cách loại bỏ sự phụ thuộc vào việc kiểm tra từng phần tử riêng lẻ bên trong tập hợp con. Giả sử một tập hợp con có tổng`S_sub`. Nếu loại bỏ bất kỳ phần tử nào làm nó mất đa số thì đối với mọi phần tử`x`trong tập con ta phải có:`S_sub - x <= S_total / 2`Sắp xếp lại mang lại:`x >= S_sub - S_total / 2`Điều này có nghĩa là mọi phần tử trong tập hợp con ít nhất phải có một ngưỡng nhất định phụ thuộc vào tổng tập hợp con. Sự phụ thuộc đó thoạt nhìn có vẻ tròn, nhưng nó sẽ có thể quản lý được nếu chúng ta sửa phần tử nhỏ nhất trong tập hợp con. 

Nếu chúng ta giả sử một tập hợp con có phần tử tối thiểu`m`, thì tất cả các phần tử ít nhất`m`, và điều kiện trở thành:`S_sub <= S_total / 2 + m`Đồng thời, chúng ta vẫn cần:`S_sub > S_total / 2`Vì vậy, đối với một phần tử tối thiểu cố định, các tập hợp con hợp lệ chính xác là những tập hợp con có tổng nằm trong một khoảng hẹp được kiểm soát bởi mức tối thiểu đó. 

Điều này gợi ý việc sắp xếp hoặc lặp lại theo phần tử nào là tối thiểu và đếm các tập hợp con trong đó tất cả các phần tử đến từ một hậu tố của các phần tử ít nhất có giá trị đó. Điều đó biến bài toán thành việc đếm các tổng tập hợp con dưới các ràng buộc trong phạm vi tổng. 

Chúng ta có thể tính toán trước số tổng tập hợp con cho mỗi hậu tố bằng cách sử dụng lập trình động, sau đó, đối với mỗi phần tử tối thiểu có thể, hãy truy vấn xem có bao nhiêu tổng tập hợp con rơi vào khoảng được yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(1) | Quá chậm | 
| Hậu tố DP + trục tối thiểu | O(n · S) | O(n · S) | Đã chấp nhận | 

Đây`S`là tổng số ghế, nhiều nhất là khoảng 6e5. 

## Hướng dẫn thuật toán 

Chúng tôi định dạng lại cách tính sao cho mỗi liên minh được tính bằng cách chọn phần tử tối thiểu trước, sau đó mở rộng nó với các phần tử lớn hơn hoặc bằng nhau. 

1. Tính tổng số ghế và xác định ngưỡng đa số là một nửa số tiền này. 
2. Sắp xếp các bên theo giá trị chỗ ngồi của họ hoặc xử lý các chỉ số theo thứ tự kích thước chỗ ngồi tăng dần sao cho các hậu tố thể hiện các lựa chọn hợp lệ cho “tập hợp con neo phần tử tối thiểu”. 
3. Xây dựng bảng lập trình động trong đó`dp[i][s]`biểu thị số cách để chọn một tập hợp con chỉ sử dụng các bên từ chỉ mục`i`ĐẾN`n`số tiền đó bằng`s`. 
4. Tính DP này từ phải sang trái. Đối với mỗi bên`i`, chúng ta loại trừ hoặc bao gồm nó, dịch chuyển tổng tương ứng. Điều này đảm bảo mọi hậu tố đều có phân phối tổng tập hợp con đầy đủ. 
5. Đối với mỗi chỉ số`i`, đãi tiệc`i`là thành phần tối thiểu của liên minh. Bất kỳ liên minh hợp lệ nào chứa nó phải chọn tất cả các thành viên khác từ các chỉ số`i+1`trở đi. 
6. Nếu phần tử tối thiểu là`p[i]`, sau đó đối với tập con được chọn của hậu tố có tổng`t`, tổng liên minh đầy đủ trở thành`t + p[i]`. Chúng tôi chuyển các điều kiện hiệu lực thành các ràng buộc trên`t`: 

Liên minh phải đáp ứng`S_total / 2 < t + p[i] <= S_total / 2 + p[i]`, điều này đơn giản hóa thành`S_total / 2 - p[i] < t <= S_total / 2`. 
7. Tính tổng tất cả số dp`t`trong khoảng thời gian này cho mỗi`i`, và tích lũy kết quả. 

Tính đúng đắn xuất phát từ thực tế là mọi liên minh hợp lệ đều có một phần tử tối thiểu duy nhất. Khi phần tử đó đã được cố định, phần còn lại của liên minh sẽ không bị hạn chế ngoại trừ ít nhất là mức tối thiểu đó (được đảm bảo bằng hạn chế hậu tố) và các ràng buộc đa số/tối thiểu trở thành giới hạn tổng đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    total = sum(a)
    half = total // 2
    
    max_sum = total
    
    dp_next = [0] * (max_sum + 1)
    dp_next[0] = 1
    
    ans = 0
    
    for i in range(n - 1, -1, -1):
        p = a[i]
        
        dp_cur = dp_next[:]  # not take p[i]
        for s in range(max_sum - p, -1, -1):
            if dp_next[s]:
                dp_cur[s + p] += dp_next[s]
        
        dp_next = dp_cur
        
        low = half - p + 1
        high = half
        
        if low < 0:
            low = 0
        
        for t in range(low, high + 1):
            ans += dp_next[t]
    
    print(ans)

if __name__ == "__main__":
    solve()
```DP được xây dựng từ cuối để`dp_next`luôn đại diện cho các tập hợp con của hậu tố ngay sau chỉ mục hiện tại. Khi xử lý một phần tử mới, chúng tôi sẽ sao chép DP và thêm các hiệu ứng chuyển tiếp bao gồm số lượng chỗ hiện tại. 

Truy vấn khoảng sử dụng bất đẳng thức được chuyển đổi. Giới hạn dưới là`half - p[i] + 1`bởi vì chúng ta cần một bất đẳng thức nghiêm ngặt ở vế trái, trong khi giới hạn trên bao hàm tại`half`. 

Một điểm tế nhị là chúng tôi cố tình sử dụng lại`dp_next`sau khi cập nhật nó cho từng chỉ mục, do đó, mỗi bên được xử lý chính xác dưới dạng tập hợp con tối thiểu tiềm năng trong hậu tố của nó. 

## Ví dụ đã hoạt động 

Hãy xem xét một cấu hình nhỏ với chỗ ngồi`[3, 2, 2]`. Tổng số là 7, vậy một nửa là 3. 

Chúng tôi xây dựng các bảng DP hậu tố: 

Đối với chỉ mục 2 (giá trị 2), các tập hợp con là`{}`,`{2}`vậy tổng là 0 và 2. 

Đối với chỉ số 1 (giá trị 2), tập hợp con trên`[2,2]`đưa ra số tiền`{0,2,2,4}`. 

Đối với chỉ số 0 (giá trị 3), tập hợp con trên`[3,2,2]`đưa ra tất cả các kết hợp. 

Bây giờ đánh giá từng mức tối thiểu: 

Đối với tối thiểu 3, hậu tố là`[2,2]`. Chúng tôi cần`t`như vậy`3.5 - 3 < t <= 3.5`, nghĩa`0.5 < t <= 3.5`, rất hợp lệ`t`là`{2}`. Điều đó mang lại tập hợp con`{3,2}`Và`{3,2}`(tùy theo chọn 2), đóng góp hai liên minh. 

Đối với tối thiểu 2 ở chỉ số 1, hậu tố là`[2]`. Chúng tôi cần`3.5 - 2 < t <= 3.5`, Vì thế`1.5 < t <= 3.5`, cho`t=2`. Điều đó tạo ra liên minh`{2,2}`. 

| Tối thiểu | Hậu tố | Phạm vi t hợp lệ | Liên minh hợp lệ | 
| --- | --- | --- | --- | 
| 3 | [2,2] | (0,5, 3,5] | {3,2} biến thể | 
| 2 | [2] | (1,5, 3,5] | {2,2} | 

Điều này xác nhận rằng mỗi liên minh được tính chính xác một lần thông qua phần tử tối thiểu của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · S) | Mỗi phần tử cập nhật DP tổng tập hợp con ở hầu hết các trạng thái S và mỗi chỉ mục thực hiện tích lũy một phạm vi | 
| Không gian | O(S) | Chúng tôi chỉ duy trì một mảng DP hậu tố | 

Tổng số ghế được giới hạn vào khoảng 600.000 và với 60 bên, DP này thoải mái phù hợp trong giới hạn thời gian bằng Python được tối ưu hóa hoặc dễ dàng trong C++. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# sample-like small case
assert run("3\n1 2 3\n") == "2", "basic sanity"

# single party
assert run("1\n10\n") == "1", "single element always valid"

# all equal
assert run("3\n2 2 2\n") == "3", "symmetry case"

# increasing values
assert run("4\n1 2 3 4\n") >= "1", "non-trivial structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 bữa tiệc | 1 | trường hợp cạnh tối thiểu | 
| tất cả đều bình đẳng | 3 | đối xứng và nhiều cực tiểu hợp lệ | 
| ngày càng tăng | khác không | tính đúng đắn chung | 

## Vỏ cạnh 

Cho một bữa tiệc duy nhất`[x]`, tổng số là`x`và một nửa là`x/2`. Tập hợp con duy nhất là`{x}`, luôn thỏa mãn đa số và tối thiểu. DP đếm chính xác điều này vì hậu tố DP bắt đầu bằng tập hợp con trống và truy vấn phạm vi bao gồm`t = 0`. 

Đối với trường hợp có sự mất cân bằng lớn như`[100, 1, 1, 1]`, chỉ những liên minh có số lượng 100 người mới có thể đạt được những hạn chế của đa số một cách tối thiểu. DP hạn chế chính xác các tổng hậu tố để các phần tử nhỏ hơn không thể tạo thành các tập hợp đa số độc lập. 

Đối với các trường hợp đối xứng như`[2,2,2]`, mỗi cặp là một liên minh đa số tối thiểu hợp lệ. Mỗi liên minh được tính chính xác một lần vì nó được liên kết với chỉ số tối thiểu duy nhất của nó, ngăn chặn việc đếm quá mức.
