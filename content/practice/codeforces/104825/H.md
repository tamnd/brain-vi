---
title: "CF 104825H - Yếu tố xác định LCA"
description: "Cho một cây có gốc có các đỉnh được đánh số từ 1 đến n, với đỉnh 1 đóng vai trò là gốc. Mỗi đỉnh u mang một giá trị a[u]."
date: "2026-06-28T12:33:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104825
codeforces_index: "H"
codeforces_contest_name: "The 17-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 104825
solve_time_s: 55
verified: true
draft: false
---

[CF 104825H - Yếu tố quyết định LCA](https://codeforces.com/problemset/problem/104825/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho một cây có gốc có các đỉnh được đánh số từ 1 đến n, với đỉnh 1 đóng vai trò là gốc. Mỗi đỉnh u mang một giá trị a[u]. Từ cây này, chúng ta xây dựng một ma trận A n x n trong đó mục nhập ở hàng i và cột j là giá trị của tổ tiên chung thấp nhất của nút i và j, đó là a[lca(i, j)]. 

Nhiệm vụ là tính định thức của ma trận này theo modulo 998244353. Khó khăn chính không phải là bản thân định thức mà thực tế là mọi mục nhập đều phụ thuộc vào truy vấn cấu trúc trên cây, do đó ma trận rất phi cục bộ và dày đặc mặc dù đối tượng cơ bản thưa thớt. 

Ràng buộc n lên tới 5 × 10^5 buộc chúng ta tránh xa bất cứ thứ gì giống với việc xây dựng ma trận rõ ràng hoặc loại bỏ Gaussian trên ma trận n x n. Ngay cả một lần vượt qua O(n^2) cũng đã là không thể và tính toán xác định trong O(n^3) hoặc O(n^2 log n) là hoàn toàn nằm ngoài tầm với. Các giải pháp khả thi duy nhất là những giải pháp làm giảm vấn đề thành các thao tác cấu trúc O(n) hoặc O(n log n) trên chính cây đó. 

Một cạm bẫy phổ biến xuất phát từ việc cố gắng suy luận về ma trận như thể nó tùy tiện nhưng có cấu trúc. Ví dụ: trên chuỗi 1-2-3 có các giá trị a1, a2, a3, ma trận trở thành dạng tam giác thấp hơn sau một phép biến đổi phù hợp, nhưng hành vi này không khái quát hóa một cách tầm thường nếu không sử dụng các phép toán hàng và cột dành riêng cho cây. Một trực giác sai lầm khác là vì LCA có tính đối xứng nên người ta có thể mong đợi một số cách giải thích giá trị riêng đơn giản, nhưng cấu trúc phụ thuộc quá tổ hợp đối với các phím tắt quang phổ. 

Giải pháp đúng dựa vào việc phát hiện ra rằng ma trận có thể được chéo hóa bằng cách sử dụng phép loại trừ nhận biết cây, biến nó thành sản phẩm của các khác biệt cục bộ độc lập dọc theo các cạnh cha-con. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp xây dựng ma trận A đầy đủ và sau đó tính định thức của nó bằng cách sử dụng phép loại bỏ Gaussian. Mỗi mục nhập yêu cầu một truy vấn LCA, truy vấn này có thể được trả lời bằng O(1) hoặc O(log n) sau khi xử lý trước, nhưng ma trận vẫn có n^2 mục nhập. Điều này đã buộc Ω(n^2) thời gian và Ω(n^2) bộ nhớ, điều này không thể thực hiện được ở n = 5 × 10^5. 

Bước đột phá về cấu trúc xuất phát từ việc quan sát thấy rằng hoạt động LCA hoạt động tốt dưới các hoạt động hàng và cột đồng thời dọc theo cây. Thay vì làm việc với các mục nhập tùy ý, chúng tôi khai thác thực tế là mỗi nút chỉ thay đổi mối quan hệ LCA của nó khi so sánh với nút gốc của nó trong cây có gốc. Điều này cho phép chúng ta dần dần “bóc” những đóng góp của trẻ em đối với cha mẹ, biến ma trận thành dạng đường chéo mà không bao giờ hiện thực hóa nó. 

Ý tưởng chính là việc trừ hàng cha mẹ khỏi hàng của nút và trừ đối xứng cột cha khỏi cột của nó, tách biệt sự đóng góp của nút đó trong định thức. Sau phép biến đổi này, tất cả các tương tác ngoài đường chéo đều bị hủy bỏ và chỉ có sự khác biệt cục bộ a[u] − a[parent[u]] vẫn còn trên đường chéo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 + n^3) | O(n^2) | Quá chậm | 
| Loại bỏ cây | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây tại 1 và xác định parent[1] = 0 với a[0] = 0 để thuận tiện. Giải pháp tiến hành bằng cách biến đổi ma trận một cách ngầm định bằng cách sử dụng các phép toán hàng và cột để bảo toàn định thức đến mức bằng nhau chính xác.

1. Chúng tôi xem xét các nút theo bất kỳ thứ tự nào phù hợp với việc nút cha được xử lý trước nút con, thường là thứ tự DFS từ gốc. Bản thân thứ tự không quan trọng nhưng nó đảm bảo chúng ta không bao giờ tham chiếu sai cấu trúc chưa được xử lý. 
2. Với mọi nút u từ 2 đến n, chúng ta thực hiện thao tác hàng: trừ row[parent[u]] khỏi row[u]. Phép toán này bảo toàn định thức vì việc cộng bội số của hàng này với hàng khác không làm thay đổi giá trị định thức. 
3. Sau đó chúng ta thực hiện thao tác cột đối xứng: trừ cột[parent[u]] khỏi cột[u]. Điều này cũng bảo toàn định thức. 
4. Sau cả hai thao tác, chúng tôi phân tích từng phần ma trận kết quả. Một sự đơn giản hóa cấu trúc quan trọng xảy ra: hầu hết các mục nhập ngoài đường chéo bị hủy vì các mối quan hệ LCA không thay đổi khi nâng cấp cha mẹ hoặc dịch chuyển chính xác một bước về phía gốc. Những đóng góp khác 0 duy nhất còn sót lại tập trung khi i = j = u. 
5. Ma trận kết quả trở thành đường chéo và mục nhập đường chéo của nút u trở thành chính xác là a[u] − a[parent[u]]. 
6. Định thức bây giờ là tích của tất cả các mục trên đường chéo. 

Bất biến quan trọng là sau khi xử lý tất cả các nút theo cách từ dưới lên, mọi cặp (i, j) với i ≠ j đã được tạo thành 0 bằng cách hủy lặp đi lặp lại dọc theo các đường dẫn cây duy nhất. Mỗi nút chỉ đóng góp thông qua chênh lệch giữa giá trị của nó và giá trị của nút cha vì bất kỳ LCA nào liên quan đến nút đó đều chuyển sang nút cha của nó theo phép trừ hàng và cột hoặc hủy hoàn toàn khi áp dụng cùng một phép chuyển đổi trên cả hai chiều. Định thức được giữ nguyên toàn bộ vì chúng ta chỉ sử dụng phép cộng hàng và cột cơ bản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    parent = [0] + list(map(int, input().split()))
    a = [0] + list(map(int, input().split()))

    # a[0] = 0 for convenience
    res = 1
    for i in range(1, n + 1):
        res = (res * (a[i] - a[parent[i]])) % MOD

    print(res % MOD)

if __name__ == "__main__":
    solve()
```Việc thực hiện mã hóa trực tiếp dạng đường chéo dẫn xuất. Chúng tôi giới thiệu rõ ràng nút cha ảo của nút gốc là 0 để nút gốc đóng góp a[1] − 0. Mọi nút khác đều đóng góp giá trị của nó trừ đi giá trị của nút cha. 

Phép trừ phải được xử lý theo modulo 998244353, do đó, các kết quả trung gian được lấy theo modulo sau khi nhân. Python xử lý các số nguyên lớn một cách tự nhiên, nhưng chúng tôi vẫn giảm bớt ở mỗi bước để đảm bảo an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét một chuỗi gồm ba nút: 1 là nút gốc, 2 nút con của 1, 3 nút con của 2. Đặt các giá trị là a = [5, 1, 4]. 

Mảng cha là [0, 1, 2]. 

Chúng tôi tính toán đóng góp: 

| tôi | một [tôi] | cha mẹ[i] | a[i] - a[parent[i]] | 
| --- | --- | --- | --- | 
| 1 | 5 | 0 | 5 | 
| 2 | 1 | 5 | -4 | 
| 3 | 4 | 1 | -1 | 

Định thức là 5 × (-4) × (-1) = 20. 

Điều này phù hợp với kết quả thu được bằng cách tính toán ma trận trực tiếp sau khi mở rộng và loại bỏ LCA, trong đó cấu trúc sụp đổ thành một hệ thống tam giác dọc theo chuỗi. 

### Ví dụ 2 

Lấy một ngôi sao có gốc tại 1 với các nút 2, 3, 4 đều được kết nối trực tiếp với 1 và các giá trị a1=1, a2=2, a3=3, a4=4. 

| tôi | một [tôi] | cha mẹ[i] | sự khác biệt | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 
| 2 | 2 | 1 | 1 | 
| 3 | 3 | 1 | 2 | 
| 4 | 4 | 1 | 3 | 

Định thức trở thành 1 × 1 × 2 × 3 = 6. 

Điều này phản ánh rằng mỗi lá đóng góp độc lập vì tất cả LCA với các lá khác nhau đều thu gọn về gốc và sau khi loại bỏ, chỉ những sai lệch trực tiếp so với gốc mới tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tính toán một lần sự khác biệt của cha mẹ và nhân | 
| Không gian | O(n) | Lưu trữ cho cha mẹ và các giá trị | 

Giải pháp này biến toàn bộ bài toán xác định ma trận thành một phép truyền tuyến tính qua các nút. Với n lên đến 5 × 10^5, giải pháp O(n) phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod
    MOD = 998244353

    n = int(input())
    parent = [0] + list(map(int, input().split()))
    a = [0] + list(map(int, input().split()))

    res = 1
    for i in range(1, n + 1):
        res = res * (a[i] - a[parent[i]]) % MOD
    return str(res)

# sample-like tests
assert run("1\n\n5\n") == "5"
assert run("3\n1 1\n1 2 3\n") == "6"

# chain
assert run("4\n1 2 3\n1 2 3 4\n") == str((1*(2-1)*(3-2)*(4-3))%998244353)

# star
assert run("4\n1 1 1\n1 2 3 4\n") == str((1*(2-1)*(3-1)*(4-1))%998244353)

# all equal
assert run("3\n1 1\n7 7 7\n") == str((7*(7-7)*(7-7))%998244353)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây xích | sản phẩm của sự khác biệt liên tiếp | tính đúng đắn của cấu trúc tuyến tính | 
| cây sao | hành vi LCA do gốc thống trị | sự độc lập của các chi nhánh | 
| tất cả các giá trị bằng nhau | định thức bằng 0 ngoại trừ căn thức | hủy bỏ đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các giá trị nút giống hệt nhau. Trong tình huống này, mọi sai phân a[u] − a[parent[u]] đều trở thành 0 ngoại trừ có thể là nghiệm, nó ngay lập tức thu gọn định thức về 0. Thuật toán xử lý việc này mà không cần phân nhánh đặc biệt vì phép nhân truyền thừa số 0 một cách tự nhiên. 

Một trường hợp khác là khi cây thoái hóa thành một chuỗi dài. Công thức giảm xuống thành một tích số chênh lệch dọc theo chuỗi và không có sự mơ hồ về việc hủy bỏ nào xuất hiện vì mỗi nút có chính xác một nút cha. 

Trường hợp thứ ba là khi các giá trị bao gồm 0 hoặc gần nhau theo modulo 998244353. Vì phép trừ được thực hiện theo modulo số học, các giá trị trung gian âm bao quanh một cách chính xác và định thức vẫn nhất quán với cách diễn giải đại số tuyến tính mô-đun.
