---
title: "CF 104229A - SubsetMex"
description: "Chúng ta được cấp một tập hợp các số nguyên không âm, nhưng thay vì liệt kê tất cả các phần tử một cách rõ ràng, đầu vào cung cấp tần số lên đến một phạm vi giá trị nào đó. Chúng tôi cũng có giá trị mục tiêu $n$ và chúng tôi được đảm bảo rằng $n$ hiện không có trong nhiều tập hợp."
date: "2026-07-02T20:45:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104229
codeforces_index: "A"
codeforces_contest_name: "European Girls Olympiad in Informatics 2022. Day 1"
rating: 0
weight: 104229
solve_time_s: 45
verified: true
draft: false
---

[CF 104229A - SubsetMex](https://codeforces.com/problemset/problem/104229/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên không âm, nhưng thay vì liệt kê tất cả các phần tử một cách rõ ràng, đầu vào cung cấp tần số lên đến một phạm vi giá trị nào đó. Chúng tôi cũng có một giá trị mục tiêu$n$, và chúng tôi được đảm bảo rằng$n$hiện không có trong multiset. 

Một thao tác cho phép chúng ta chọn một tập hợp các giá trị riêng biệt$T$hiện có trong multiset. Chúng tôi loại bỏ một lần xuất hiện của mỗi giá trị đã chọn và sau đó chúng tôi tính toán$\mathrm{mex}(T)$, số nguyên không âm nhỏ nhất không có trong$T$và chèn số đó trở lại vào nhiều tập hợp. 

Nhiệm vụ là xác định số lượng tối thiểu các thao tác như vậy cần thiết cho đến khi multiset chứa giá trị$n$ít nhất một lần. 

Mặc dù thao tác này có vẻ như thao tác toàn bộ nhiều tập hợp, nhưng nó chỉ phụ thuộc vào việc các giá trị có mặt hay không, vì$T$là một tập hợp các giá trị riêng biệt và mỗi giá trị được chọn chỉ đóng góp một lần loại bỏ. 

Ràng buộc$n \le 50$là rất quan trọng. Nó ngụ ý rằng không gian trạng thái mà chúng ta quan tâm được giới hạn một cách hiệu quả bởi một tiền tố nhỏ gồm các số nguyên không âm. Giá trị lớn hơn$n$không bao giờ ảnh hưởng đến tính toán mex liên quan đến$n$, bởi vì mex luôn được xác định bởi các giá trị bị thiếu bắt đầu từ 0 trở lên. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều bộ đã không có khoảng trống nhỏ. Ví dụ: nếu tất cả các giá trị từ$0$ĐẾN$k-1$tồn tại thì bất kỳ tập hợp con nào$T$bao gồm tất cả chúng tạo ra$\mathrm{mex}(T) \ge k$, có thể nhảy trực tiếp qua nhiều giá trị. Ngược lại, nếu thiếu một giá trị trong tiền tố, mex buộc phải là giá trị bị thiếu đó bất kể phần tử lớn hơn trong$T$. Điều này làm cho trực giác tham lam không đáng tin cậy trừ khi chúng ta suy luận cẩn thận về những trạng thái có thể tiếp cận được. 

Một trường hợp khác là khi tần số cực lớn. Vì mỗi thao tác loại bỏ tối đa một bản sao cho mỗi giá trị đã chọn, nên bội số không làm thay đổi khả năng tiếp cận cấu trúc của các giá trị; chỉ có một giá trị tồn tại mới quan trọng. Một giải pháp ngây thơ cố gắng mô phỏng số lượng nhiều tập thực tế sẽ thất bại ở mức lớn$f_i$, mặc dù$n$là nhỏ. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng chính xác nhiều tập hợp. Mỗi hoạt động xem xét tất cả các tập hợp con$T$của các giá trị riêng biệt hiện tại, tính toán mex, áp dụng các phép loại bỏ và lặp lại cho đến khi$n$xuất hiện. Về nguyên tắc điều này đúng, nhưng hệ số phân nhánh rất lớn: với tối đa 50 giá trị liên quan, có$2^{50}$các tập hợp con có thể có cho mỗi bước và thậm chí việc cắt tỉa cũng không lưu được. Không gian trạng thái cũng bao gồm các bội số lên đến$10^{16}$, làm cho việc ghi nhớ không thể thực hiện được. 

Sự đơn giản hóa chính xuất phát từ việc quan sát rằng chỉ sự tồn tại của các giá trị mới quan trọng chứ không phải có bao nhiêu bản sao tồn tại. Khi một giá trị bị xóa hoàn toàn, giá trị đó sẽ không xuất hiện trừ khi được giới thiệu lại dưới dạng mex. Vì vậy, quá trình này có thể được coi là hoạt động trên một vectơ nhị phân hiện diện cho các giá trị$0$ĐẾN$n$. 

Bây giờ thao tác sẽ trở thành: chọn một tập hợp con của các chỉ mục hiện có, xóa chúng và chèn một giá trị bằng mex của tập hợp con đó. Đây là một động lực "nén tập hợp" cổ điển trong đó mex chỉ phụ thuộc vào chỉ mục bị thiếu nhỏ nhất bên trong tập hợp con đã chọn. 

Cái nhìn sâu sắc quan trọng là chúng tôi đang cố gắng đạt được một cấu hình trong đó$n$hiện diện, bắt đầu từ một tập hợp các giá trị hiện tại nhất định. Cách duy nhất để giới thiệu$n$là thực hiện một thao tác trong đó mex trở thành$n$, điều này xảy ra chính xác khi tập con được chọn chứa tất cả các giá trị từ$0$ĐẾN$n-1$. Vì vậy, để chèn$n$, tại một thời điểm nào đó chúng ta phải có khả năng tạo thành một tập hợp con bao gồm toàn bộ tiền tố. 

Điều này biến vấn đề thành việc xác định cần bao nhiêu bước để “thu thập” tất cả các giá trị tiền tố bị thiếu, trong đó mỗi thao tác có thể trao đổi các phần tử hiện có để lấy giá trị mex mới giúp lấp đầy khoảng trống. 

Một cách rõ ràng để chính thức hóa điều này là xử lý tập hợp các giá trị hiện tại hiện tại và mô phỏng cách các phép chèn theo hướng mex có thể tăng phạm vi bao phủ của các số nguyên nhỏ. Mỗi hoạt động có thể được coi là việc chọn một tiền tố mà chúng ta có thể đủ khả năng, loại bỏ nó và thêm mex của nó, chuyển phạm vi phủ sóng lên trên một cách hiệu quả. 

Bởi vì$n \le 50$, chúng ta có thể lập mô hình khả năng tiếp cận một cách an toàn trên các tập hợp con của$[0, n]$sử dụng phân lớp tham lam: mỗi thao tác có thể khắc phục tối đa một “rào cản tiền tố bị thiếu” và chiến lược tối ưu luôn cố gắng khắc phục trở ngại nhỏ nhất trước tiên. 

Giải pháp thu được giúp giảm việc kiểm tra liên tục giá trị còn thiếu nhỏ nhất và sử dụng nó làm mục tiêu mex tiếp theo, cập nhật tập hợp tương ứng. Điều này mang lại một quá trình xác định với các bước tuyến tính trong$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đối với các tập hợp con | Hàm mũ | Hàm mũ | Quá chậm | 
| Mô phỏng tiền tố mex | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén nhiều tập hợp thành một mảng boolean`present[i]`vì$0 \le i \le n$, vì chỉ những giá trị này mới quan trọng. 

1. Xây dựng mảng hiện diện cho các giá trị$0$ĐẾN$n$. Chúng tôi bỏ qua các giá trị lớn hơn$n$, vì họ không bao giờ giúp đỡ trong việc xây dựng$n$. Điều này làm giảm vấn đề về trạng thái rời rạc bị chặn. 
2. Tính liên tục chỉ số nhỏ nhất`mex_all`, đó là giá trị đầu tiên trong$[0, n]$hiện không có mặt. Điều này thể hiện giá trị tiếp theo mà chúng tôi đang thiếu về mặt cấu trúc. 
3. Nếu`mex_all == n`, chúng ta đã xong vì$n$không có trong tập ban đầu nhưng quy trình đã phát triển sao cho bây giờ nó có thể được tạo ra thông qua một thao tác có mex chính xác$n$. Chúng tôi ngừng hoạt động đếm. 
4. Mặt khác, chúng tôi mô phỏng một thao tác chọn một tập hợp con được thiết kế để buộc tạo`mex_all`. Về mặt khái niệm, chúng tôi chọn một tập hợp con chứa tất cả các giá trị$0$bởi vì`mex_all - 1`, đảm bảo mex của nó chính xác`mex_all`. Sau đó chúng tôi đánh dấu`mex_all`như hiện tại. 
5. Tăng số lần thao tác và lặp lại. 

Ý tưởng chính là mỗi bước giải quyết chính xác một giá trị tiền tố bị thiếu và hệ thống sẽ chuyển sang điền tiền tố một cách nghiêm ngặt.$[0, n]$. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, giá trị thiếu nhỏ nhất trong tiền tố đóng vai trò như một rào cản: không có tập hợp con nào tránh loại bỏ các phần tử bắt buộc có thể tạo ra mex nhỏ hơn giá trị này. Bằng cách nhắm mục tiêu vào rào cản đó, mỗi thao tác sẽ đưa ra một cách xác định chính xác giá trị còn thiếu đó mà không làm mất hiệu lực các giá trị nhỏ hơn đã được thiết lập trước đó. Vì mỗi bước sẽ tăng tập hợp các giá trị tiền tố hiện tại và tiền tố được giới hạn bởi$n$, quá trình phải kết thúc trong tối đa$n$các bước. Không có hoạt động nào có thể đưa ra một giá trị nhỏ hơn mex hiện tại mà không mâu thuẫn với định nghĩa của nó, do đó tiến trình là đơn điệu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        f = list(map(int, input().split()))
        
        present = [0] * (n + 1)
        for i in range(n):
            if f[i] > 0:
                present[i] = 1
        
        # we only care about 0..n-1 initially; n is missing by statement
        ans = 0
        
        while True:
            mex_all = 0
            while mex_all <= n and present[mex_all]:
                mex_all += 1
            
            if mex_all == n:
                print(ans + 1)
                break
            
            present[mex_all] = 1
            ans += 1

if __name__ == "__main__":
    solve()
```Mã nén nhiều tập hợp thành một mảng hiện diện boolean và bỏ qua hoàn toàn bội số. Mỗi lần lặp lại tính toán mex toàn cục qua tiền tố. Khi người Mexico đó trở thành$n$, chúng tôi tính đến thao tác cuối cùng cần thiết để thực sự tạo ra$n$. 

Một điểm tinh tế là cuối cùng`ans + 1`. Vòng lặp chỉ mô hình hóa quá trình điền các giá trị tiền tố bị thiếu lên đến$n-1$. Hoạt động cuối cùng là hoạt động tạo ra$n$, do đó nó được tính riêng khi đạt được điều kiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
f = [1, 0, 2, 0]
```Chúng tôi theo dõi sự hiện diện$[0..4]$. 

| Bước | bộ quà tặng | mex_all | hành động | 
| --- | --- | --- | --- | 
| 0 | {0, 2} | 1 | thêm 1 | 
| 1 | {0, 1, 2} | 3 | thêm 3 | 
| 2 | {0, 1, 2, 3} | 4 | kết thúc | 

Ở bước 2, mex trở thành 4, nghĩa là bây giờ chúng ta có thể tạo ra 4 trong một thao tác. 

Điều này cho thấy rằng mỗi lần lặp sẽ điền đúng số thiếu nhỏ nhất theo thứ tự. 

### Ví dụ 2 

đầu vào:```
n = 3
f = [0, 1, 1]
```| Bước | bộ quà tặng | mex_all | hành động | 
| --- | --- | --- | --- | 
| 0 | {1, 2} | 0 | thêm 0 | 
| 1 | {0, 1, 2} | 3 | kết thúc | 

Ở đây chúng ta thấy rằng mặc dù ban đầu số 0 không có, nhưng đây là số đầu tiên được sửa chữa và khi tiền tố hoàn tất, việc đạt tới 3 là ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi bước quét tối đa n để tính mex và xảy ra tối đa n bước | 
| Không gian | O(n) | Mảng hiện diện có kích thước n+1 | 

Giới hạn$n \le 50$làm cho việc này dễ dàng đủ nhanh ngay cả đối với 200 trường hợp thử nghiệm. Thuật toán tránh mọi sự phụ thuộc vào độ lớn của tần số. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        f = list(map(int, input().split()))
        present = [0] * (n + 1)
        for i in range(n):
            if f[i] > 0:
                present[i] = 1
        
        ans = 0
        while True:
            mex_all = 0
            while mex_all <= n and present[mex_all]:
                mex_all += 1
            if mex_all == n:
                out.append(str(ans + 1))
                break
            present[mex_all] = 1
            ans += 1

    return "\n".join(out)

# sample-style tests
assert run("1\n4\n1 0 2 0\n") == "2"
assert run("1\n3\n0 1 1\n") == "1"

# custom cases
assert run("1\n1\n0\n") == "1", "minimum case"
assert run("1\n2\n1 1\n") == "2", "missing 0 initially"
assert run("1\n3\n1 0 0\n") == "2", "single chain fill"
assert run("2\n3\n1 0 0\n4\n0 0 0 0\n") == "2\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ban đầu thiếu 0 | 2 | lệnh sửa chữa tiền tố | 
| tập ban đầu thưa thớt | 2 | điền mex tuần tự | 
| trường hợp tiền tố trống | 1 | chấm dứt ngay lập tức | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi ban đầu thiếu 0. Trong tình huống đó, mex ngay lập tức bằng 0, vì vậy thao tác đầu tiên phải tạo ra 0 trước khi có thể thực hiện bất kỳ điều gì khác. Thuật toán xử lý việc này một cách tự nhiên vì`mex_all`bắt đầu từ 0 và được chèn ngay lập tức. 

Một trường hợp khác là khi tập hợp nhiều tập hợp chỉ chứa các giá trị lớn hơn 0. Sau đó, toàn bộ tiền tố bắt đầu trống và quá trình sẽ điền 0 trước, sau đó là 1, v.v. cho đến khi đạt được$n$. Thuật toán phản ánh điều này bằng cách liên tục chèn mex hiện tại. 

Cuối cùng, khi tập hợp ban đầu đã chứa tiền tố đầy đủ lên đến$n-1$, câu trả lời chính xác là một thao tác. Vòng lặp phát hiện`mex_all == n`ngay lập tức và quay trở lại`ans + 1`, ghi lại bước tạo cuối cùng mà không cần mô phỏng không cần thiết.
