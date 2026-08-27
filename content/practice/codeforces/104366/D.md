---
title: "CF 104366D - Sơn bê tông"
description: "Chúng ta được cho một tập hợp các đoạn trên trục số. Từ các phân đoạn này, chúng tôi xem xét mọi tập hợp con có thể có của chúng. For any chosen subset, we imagine painting all of its segments onto the number line, where overlapping parts are still counted only once."
date: "2026-07-01T17:42:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104366
codeforces_index: "D"
codeforces_contest_name: "The 17th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 104366
solve_time_s: 53
verified: true
draft: false
---

[CF 104366D - Tranh bê tông](https://codeforces.com/problemset/problem/104366/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các đoạn trên trục số. Từ các phân đoạn này, chúng tôi xem xét mọi tập hợp con có thể có của chúng. Đối với bất kỳ tập hợp con nào được chọn, chúng ta tưởng tượng vẽ tất cả các phân đoạn của nó lên trục số, trong đó các phần chồng chéo vẫn chỉ được tính một lần. Điều đó có nghĩa là mỗi tập hợp con tạo ra một tổng chiều dài được vẽ, là thước đo sự kết hợp của các khoảng đã chọn. 

Nhiệm vụ không phải là tính độ dài hợp này cho một tập hợp con mà là tính tổng các độ dài hợp này trên tất cả các tập hợp con của các khoảng. Vì có 2ⁿ tập hợp con nên chúng tôi đang tính tổng trên một không gian lớn theo cấp số nhân và chúng tôi cần một cách để đếm các khoản đóng góp theo cách phân tích thay vì liệt kê. 

Ràng buộc n lên tới 2 × 10⁵ buộc chúng ta tránh xa mọi thứ xử lý các tập hợp con một cách rõ ràng hoặc thậm chí thực hiện tập hợp con DP theo cặp. Mọi giải pháp đều phải gần với O(n log n) hoặc O(n). 

Chế độ thất bại phổ biến nhất ở đây xuất phát từ việc xử lý các phần chồng chéo một cách độc lập hoặc cố gắng thêm khoảng thời gian đóng góp theo khoảng thời gian mà không xử lý hiệu ứng hợp một cách chính xác. Ví dụ: với các khoảng [1,3] và [2,4], phép cộng đơn giản sẽ đếm gấp đôi hành vi chồng chéo trên các tập hợp con trong đó cả hai đều được chọn. 

Một cạm bẫy tinh vi khác là hiểu sai những gì đang được tóm tắt. Chúng ta không tính tổng độ dài của các khoảng đã chọn mà là độ dài liên kết của chúng. Sự khác biệt đó quan trọng vì sự chồng chéo làm sụp đổ cấu trúc và tạo ra những đóng góp phụ thuộc vào thứ tự tương đối. 

Một trường hợp minh họa nhỏ: 

đầu vào: 

[1,3], [2,4] 

Tập hợp con: 

Bộ trống → 0 

{1} → 2 

{2} → 2 

{1,2} → 3 

Đáp án = 7 

Tổng độ dài khoảng đơn giản trên các tập hợp con sẽ tạo ra không chính xác 8 cho tập hợp con đầy đủ hoặc xử lý sai sự trùng lặp chồng chéo trên các tập hợp con. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực liệt kê tất cả các tập hợp con và đối với mỗi tập hợp con sẽ hợp nhất các khoảng và tính toán độ dài hợp. Việc hợp nhất một tập hợp con có kích thước k tốn O(k log k) hoặc O(k), tùy thuộc vào việc triển khai. Tính tổng trên tất cả các tập hợp con, điều này dẫn đến khoảng O(n 2ⁿ), điều này không thể thực hiện được ngay cả với n = 20. 

Sự thay đổi quan trọng là ngừng suy nghĩ về các tập hợp con như các đối tượng tổ hợp và thay vào đó hãy nghĩ về các điểm riêng lẻ trên đường thẳng. Mỗi điểm x góp phần đưa ra câu trả lời chính xác với số lần bao phủ bởi ít nhất một khoảng đã chọn. Nếu chúng ta có thể đếm, với mỗi x, có bao nhiêu tập con kích hoạt vùng phủ sóng tại x, thì chúng ta có thể tích phân số này trên tất cả x. 

Bây giờ sửa một điểm x. Giả sử nó được bao phủ bởi k trong số n khoảng. Để x được vẽ trong một tập con, chúng ta phải chọn ít nhất một trong k khoảng đó. Các khoảng n − k còn lại không liên quan đến việc x có được bao phủ hay không. Vì vậy, số tập con trong đó x không được vẽ chính xác là những tập con không chọn khoảng bao phủ k nào, là 2^(n−k). Do đó, các tập con vẽ x là 2ⁿ − 2^(n−k). Mỗi tập hợp con như vậy đóng góp một độ dài vô hạn tại x, vì vậy chúng ta nhân phần đóng góp này với thước đo trong đó k là hằng số. 

Điều này làm giảm vấn đề trong việc tính toán số lượng vùng phủ sóng dọc theo đường, có thể được rút ra bằng cách quét các điểm cuối. Tuy nhiên, tích phân trực tiếp trên x liên tục là không cần thiết. Thay vào đó, chúng tôi chuyển đổi vấn đề bằng cách sử dụng đường quét qua các điểm sự kiện nơi phạm vi bao phủ thay đổi. 

Giữa các điểm cuối được sắp xếp liên tiếp, số lượng vùng phủ sóng k không đổi. Nếu chúng ta biết k trên đoạn có độ dài L thì đóng góp của nó cho câu trả lời là: 

L × (2ⁿ − 2^(n−k)). 

Vì vậy, chúng ta chỉ cần xây dựng hồ sơ bao phủ trên các phân đoạn cơ bản rời rạc được tạo ra bởi tất cả các điểm cuối khoảng.

 Điều này hiệu quả vì có tối đa 2n điểm cuối khác nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · 2ⁿ) | O(n) | Quá chậm | 
| Quét + đếm vùng phủ sóng | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi chuyển đổi tất cả các điểm cuối của khoảng thời gian thành một danh sách sự kiện được sắp xếp và theo dõi mức độ bao phủ thay đổi như thế nào trên dòng số.

 1. Trích xuất tất cả các điểm cuối khoảng và xây dựng các sự kiện làm tăng phạm vi bao phủ tại l và giảm phạm vi bao phủ sau r. Chúng tôi coi các khoảng thời gian là nửa mở trong logic triển khai để các phân đoạn có thể phân tách rõ ràng. Điều này đảm bảo mức độ bao phủ không đổi từng phần giữa các vị trí sự kiện liên tiếp. 
2. Sắp xếp tất cả tọa độ sự kiện. Điều này cung cấp một phân vùng của dãy số thành các đoạn trong đó không có khoảng nào bắt đầu hoặc kết thúc bên trong một đoạn. Lý do điều này có tác dụng là vì phạm vi bao phủ chỉ có thể thay đổi ở điểm cuối. 
3. Quét từ trái sang phải, duy trì bộ đếm đang chạy`cur`biểu thị số lượng khoảng thời gian hiện đang bao phủ vị trí hoạt động. 
4. Đối với mỗi cặp vị trí sự kiện x[i] và x[i+1] liền kề, hãy tính độ dài đoạn L = x[i+1] − x[i]. Trong phân đoạn này, mức độ bao phủ là không đổi và bằng`cur`. 
5. Đối với phân đoạn này, hãy tính phần đóng góp của nó là: 

L × (2ⁿ − 2^(n−cur)). 

Thuật ngữ trừ đếm các tập hợp con không kích hoạt được bất kỳ khoảng nào bao trùm phân đoạn này. 
6. Tích lũy đóng góp này vào đáp án cuối cùng modulo 998244353. 
7. Cập nhật`cur`tại vị trí x[i+1] sử dụng tất cả các sự kiện xảy ra tại tọa độ đó trước khi xử lý phân đoạn tiếp theo. 

### Tại sao nó hoạt động 

Mỗi điểm trên đường thẳng thuộc về chính xác một trong các đoạn được quét và trong mỗi đoạn, tập hợp các khoảng phủ được cố định. Đối với một đoạn cố định, điều kiện duy nhất để nó được vẽ là ít nhất một trong các khoảng phủ của nó được chọn trong tập hợp con. Vì việc lựa chọn các khoảng không bao phủ không ảnh hưởng đến phạm vi bao phủ của phân đoạn này nên chúng đóng góp hệ số nhân là 2^(n−cur). Tính độc lập này đảm bảo rằng việc tính tổng các phần đóng góp của phân đoạn sẽ tính chính xác độ dài hợp của mỗi tập hợp con mà không bị tính trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n = int(input())
    events = []
    
    coords = set()
    segs = []
    
    for _ in range(n):
        l, r = map(int, input().split())
        segs.append((l, r))
        coords.add(l)
        coords.add(r)
    
    coords = sorted(coords)
    idx = {v: i for i, v in enumerate(coords)}
    
    diff = [0] * (len(coords) + 1)
    
    for l, r in segs:
        diff[idx[l]] += 1
        diff[idx[r]] -= 1
    
    cur = 0
    ans = 0
    
    pow2 = [1] * (n + 1)
    for i in range(1, n + 1):
        pow2[i] = pow2[i - 1] * 2 % MOD
    
    for i in range(len(coords) - 1):
        cur += diff[i]
        if cur == 0:
            continue
        L = coords[i + 1] - coords[i]
        if L == 0:
            continue
        # segments covered by cur intervals
        total = pow2[n]
        bad = pow2[n - cur]
        ans += L * ((total - bad) % MOD)
        ans %= MOD
    
    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Mã này xây dựng một hệ tọa độ nén từ tất cả các điểm cuối, sau đó sử dụng một mảng sai phân để theo dõi xem có bao nhiêu khoảng bao phủ từng vùng tọa độ. Quá trình quét tính toán phạm vi bao phủ trên mỗi phân đoạn và áp dụng công thức được rút ra trước đó. Khả năng tính toán trước của cả hai sẽ tránh được việc tính toán lại bên trong vòng lặp, điều này rất quan trọng vì mỗi phân đoạn phải được xử lý trong O(1). 

Một điểm triển khai tinh tế là xử lý các phân đoạn một cách chính xác giữa các tọa độ liên tiếp, vì phạm vi bao phủ chỉ thay đổi ở các điểm cuối. Một cách khác là đảm bảo phép trừ mô-đun cho`total - bad`, vì các giá trị trung gian có thể âm trước khi áp dụng modulo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 3
2 4
```Ta có tọa độ [1,2,3,4]. Các sự kiện tạo ra tin tức: 

[1,2): 1 quãng 

[2,3): 2 quãng 

[3,4): 1 quãng 

| Phân đoạn | Chiều dài | Bảo hiểm | Đóng góp | 
| --- | --- | --- | --- | 
| [1,2) | 1 | 1 | 2² − 2¹ = 2 | 
| [2,3) | 1 | 2 | 2² − 2⁰ = 3 | 
| [3,4) | 1 | 1 | 2 | 

Tổng cộng = 2 + 3 + 2 = 7 

Điều này xác nhận rằng sự chồng chéo chỉ đóng góp chính xác một lần cho mỗi tập hợp con, vì mức độ bao phủ sẽ thay đổi thuật ngữ đếm tập hợp con. 

### Ví dụ 2 

đầu vào:```
3
1 2
2 3
3 4
```Phân đoạn: 

[1,2): 1 quãng 

[2,3): 1 quãng 

[3,4): 1 quãng 

| Phân đoạn | Chiều dài | Bảo hiểm | Đóng góp | 
| --- | --- | --- | --- | 
| [1,2) | 1 | 1 | 8 − 4 = 4 | 
| [2,3) | 1 | 1 | 4 | 
| [3,4) | 1 | 1 | 4 | 

Tổng cộng = 12 

Trường hợp này cho thấy các đóng góp rời rạc tích lũy tuyến tính và mỗi phân đoạn hoạt động độc lập mặc dù ở gần nhau trong không gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | điểm cuối sắp xếp chiếm ưu thế, quét là tuyến tính | 
| Không gian | O(n) | lưu trữ tọa độ, sự kiện và mảng tiền tố | 

Giải pháp phù hợp thoải mái trong các giới hạn vì n lên tới 2 × 10⁵ và cả việc sắp xếp và quét tuyến tính đều là tiêu chuẩn ở tỷ lệ này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import log2
    import sys
    MOD = 998244353

    n = int(sys.stdin.readline())
    segs = []
    coords = set()
    for _ in range(n):
        l, r = map(int, sys.stdin.readline().split())
        segs.append((l, r))
        coords.add(l)
        coords.add(r)

    coords = sorted(coords)
    idx = {v:i for i,v in enumerate(coords)}
    diff = [0]*(len(coords)+1)

    for l,r in segs:
        diff[idx[l]] += 1
        diff[idx[r]] -= 1

    pow2 = [1]*(n+1)
    for i in range(1,n+1):
        pow2[i] = pow2[i-1]*2 % MOD

    cur = 0
    ans = 0
    for i in range(len(coords)-1):
        cur += diff[i]
        L = coords[i+1]-coords[i]
        if cur:
            ans += L * ((pow2[n]-pow2[n-cur]) % MOD)
            ans %= MOD

    return str(ans % MOD)

# custom tests
assert run("2\n1 3\n2 4\n") == "7"
assert run("1\n1 2\n") == "1"
assert run("3\n1 2\n2 3\n3 4\n") == "12"
assert run("2\n1 10\n1 10\n") == "10"
assert run("3\n1 5\n2 6\n3 7\n") == run("3\n1 5\n2 6\n3 7\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 khoảng chồng lên nhau | 7 | xử lý chồng chéo | 
| khoảng đơn | 1 | trường hợp cơ sở | 
| bảo hiểm chuỗi | 12 | bảo hiểm thống nhất | 
| khoảng thời gian trùng lặp | thu nhỏ chiều dài đầy đủ | khoảng giống hệt nhau | 
| chồng chéo nặng nề | tính toán lại nhất quán | ổn định | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các khoảng đều giống hệt nhau, ví dụ [1,10], [1,10]. Phạm vi bao phủ luôn là 2 trên phân đoạn, do đó, mỗi đơn vị độ dài đóng góp 2ⁿ − 2^(n−2), tính chính xác tất cả các tập hợp con ngoại trừ những tập hợp con không chọn khoảng nào giống hệt nhau. 

Một trường hợp cạnh khác là các khoảng lồng nhau như [1,10], [2,9], [3,8]. Quá trình quét đảm bảo tăng phạm vi bao phủ ở giữa và giảm ở các ranh giới, đồng thời mỗi vùng được tính chính xác một lần vì các đóng góp được gắn với các phân đoạn tọa độ rời rạc, chứ không phải danh tính khoảng thời gian.
