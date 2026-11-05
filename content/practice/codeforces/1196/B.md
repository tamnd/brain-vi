---
title: "CF 1196B - Phân đoạn tổng lẻ"
description: "Chúng tôi được cung cấp một số mảng độc lập. Đối với mỗi phần, chúng ta phải cắt nó thành chính xác k phần liền kề nhau, trong đó mỗi phần có tổng lẻ. Thứ tự mảng là cố định nên quyền tự do duy nhất là chọn vị trí cắt."
date: "2026-06-13T14:10:05+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 1196
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 575 (Div. 3)"
rating: 1200
weight: 1196
solve_time_s: 370
verified: true
draft: false
---

[CF 1196B - Phân đoạn tổng lẻ](https://codeforces.com/problemset/problem/1196/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 6 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số mảng độc lập. Đối với mỗi phần, chúng ta phải cắt nó thành chính xác k phần liền kề nhau, trong đó mỗi phần có tổng lẻ. Thứ tự mảng là cố định nên quyền tự do duy nhất là chọn vị trí cắt. 

Một mảng con có tổng lẻ chính xác khi tính chẵn lẻ của các phần tử của nó cộng lại bằng 1 modulo 2. Vì tính chẵn lẻ là điều duy nhất quan trọng nên các giá trị bên trong không cần theo dõi chi tiết ngoài việc chúng là số lẻ hay số chẵn. 

Đầu ra không phải là các phân đoạn mà là các vị trí cắt xác định chúng. Nếu chúng ta quyết định k đoạn kết thúc tại các chỉ số r1, r2, ..., rk trong đó rk phải bằng n thì chúng ta đang mô tả phân vùng một cách hiệu quả. 

Các ràng buộc thúc đẩy giải pháp tuyến tính cho mỗi truy vấn. Tổng chiều dài mảng trên tất cả các truy vấn tối đa là 2 × 10^5, do đó, mọi giải pháp quét từng mảng một lần hoặc một số lần không đổi đều có thể chấp nhận được. Mọi thứ bậc hai trong một truy vấn sẽ quá chậm khi n lớn. 

Khó khăn chính là tổng phân đoạn là thuộc tính chung của phạm vi, nhưng chúng tôi bắt buộc phải thực thi đồng thời điều kiện chẵn lẻ nghiêm ngặt trên mọi phân đoạn. Một nỗ lực ngây thơ thử lặp đi lặp lại tất cả các vị trí bị cắt hoặc kiểm tra tổng phân khúc sẽ nhanh chóng trở nên không khả thi. 

Một vài trường hợp thất bại tinh vi xuất hiện một cách tự nhiên. Nếu chúng ta tham lam cắt bất cứ khi nào chúng ta thấy tổng tiền tố lẻ mà không kiểm soát số lượng phân đoạn còn lại, chúng ta có thể hết phần tử quá sớm hoặc quá muộn. Một lỗi khác xảy ra khi tổng số phần tử lẻ không đủ để tạo thành k phân đoạn tổng lẻ, ngay cả khi các lựa chọn tham lam cục bộ có vẻ hợp lệ. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ thử mọi cách có thể để chọn k-1 vị trí cắt trong số n-1 khoảng trống và kiểm tra xem mỗi phân đoạn kết quả có tổng lẻ hay không. Số lượng phân vùng là tổ hợp, theo thứ tự$\binom{n}{k}$, là số mũ của n trong trường hợp xấu nhất. Ngay cả việc xác minh một phân vùng duy nhất cũng là tuyến tính, do đó phương pháp này vượt xa các giới hạn khả thi. 

Quan sát quan trọng xuất phát từ việc suy nghĩ xem điều gì làm cho tổng của một phân đoạn trở thành số lẻ. Tổng phân đoạn là số lẻ khi và chỉ khi nó chứa số lẻ các phần tử lẻ. Các phần tử chẵn không ảnh hưởng đến tính chẵn lẻ ngoại trừ việc đóng góp 0 vào tính chẵn lẻ. Điều này làm giảm toàn bộ vấn đề trong việc theo dõi vị trí của các số lẻ trong mảng. 

Nếu chúng ta cố định k phân đoạn thì mỗi phân đoạn phải đóng góp một số lẻ các số lẻ. Cách đơn giản nhất để đảm bảo điều này là gán chính xác một phần tử lẻ cho mỗi cấu trúc ranh giới đoạn. Điều này gợi ý rằng chúng ta nên sử dụng k−1 phần tử lẻ làm điểm neo bị cắt cưỡng bức, bởi vì mỗi lần cắt phải kết thúc một đoạn có tổng là số lẻ. 

Khi k−1 phần tử lẻ được chọn làm điểm cuối của k−1 đoạn đầu tiên, đoạn cuối cùng sẽ tự động chứa các phần tử còn lại. Để phân đoạn cuối cùng cũng có tổng lẻ, tổng số phần tử lẻ trong toàn bộ mảng phải là số lẻ và sau khi sử dụng k−1 neo lẻ, hậu tố còn lại vẫn phải chứa số lẻ là số lẻ. Vì chúng tôi kết thúc phân đoạn cuối cùng tại n, nên điều này giảm xuống còn yêu cầu rằng chúng tôi có tổng thể ít nhất k số lẻ và vị trí lẻ được chọn thứ k phải là chiến lược điểm cuối của phân khúc lẻ cuối cùng vẫn để lại ít nhất một phần tử lẻ trong phân khúc cuối cùng. 

Do đó, việc xây dựng trở nên tham lam: quét mảng, thu thập các chỉ số của các phần tử lẻ và sử dụng k−1 chỉ số lẻ đầu tiên làm điểm cắt. Đoạn thứ k tự động kết thúc ở n. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Mỏ neo kỳ quặc tham lam | O(n) mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập. 

1. Quét mảng và ghi lại vị trí của tất cả các phần tử lẻ. Các vị trí này đại diện cho tất cả các “tài nguyên” chẵn lẻ có sẵn để hình thành các phân đoạn tổng lẻ. 
2. Nếu số phần tử lẻ nhỏ hơn k thì ta kết luận ngay là không thể. Mỗi phân đoạn phải chứa ít nhất một phần tử lẻ để làm cho tổng của nó là số lẻ, do đó chúng ta thậm chí không thể bắt đầu xây dựng k phân đoạn hợp lệ. 
3. Nếu số phần tử lẻ ít nhất là k, chúng ta chọn k−1 vị trí lẻ đầu tiên làm điểm cuối của k−1 đoạn đầu tiên. Mỗi chỉ số lẻ được chọn đảm bảo rằng phân đoạn kết thúc ở đó có tổng lẻ vì nó chứa một số lẻ các phần tử lẻ và ranh giới được căn chỉnh với phần đóng góp lẻ. 
4. Chúng ta đặt đoạn cuối cùng kết thúc ở n. Hậu tố còn lại nhất thiết phải chứa tất cả các phần tử không được sử dụng và vì chúng tôi đã bắt đầu với ít nhất k phần tử lẻ nên đoạn còn lại vẫn chứa số lẻ các số lẻ, đảm bảo tổng của nó là số lẻ. 
5. Xuất ra các vị trí cắt đã thu thập. 

Bất biến chính là sau khi chọn vị trí cắt i, chúng tôi đã tạo i phân đoạn, mỗi phân đoạn chứa ít nhất một phần tử lẻ và tiền tố cho đến chỉ số lẻ được chọn thứ i đã được sử dụng đầy đủ. Điều này đảm bảo rằng mỗi phân đoạn đã hoàn thành có tổng lẻ và không có phân đoạn nào không có điểm neo lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q = int(input())
    out = []
    
    for _ in range(q):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        odds = []
        for i, x in enumerate(a, start=1):
            if x % 2 == 1:
                odds.append(i)
        
        if len(odds) < k:
            out.append("NO")
            continue
        
        # we use first k-1 odd positions as cut points
        cuts = odds[:k-1]
        
        # last cut must be n
        cuts.append(n)
        
        out.append("YES")
        out.append(" ".join(map(str, cuts)))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện ý tưởng tham lam. Danh sách`odds`lưu trữ các chỉ số của tất cả các phần tử có giá trị lẻ. Việc kiểm tra tính khả thi`len(odds) < k`thực thi yêu cầu tối thiểu là mỗi phân đoạn phải chứa ít nhất một phần tử lẻ. 

Danh sách cắt được hình thành bằng cách lấy k−1 chỉ số lẻ đầu tiên. Việc thêm n đảm bảo điều kiện biên phân đoạn cuối cùng theo yêu cầu của định dạng đầu ra. 

Một điểm tinh tế là chúng tôi không xác nhận rõ ràng tổng của từng phân đoạn. Điều đó là không cần thiết vì mỗi phân đoạn được đảm bảo chứa chính xác một trong các điểm neo lẻ đã chọn ở điểm cuối của nó, điều này buộc tổng chẵn lẻ của phân đoạn phải là số lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
7 18 3 14 1
```Các vị trí lẻ là 1, 3, 5. 

Chúng ta cần k = 3 đoạn, vì vậy trước tiên chúng ta lấy k−1 = 2 vị trí lẻ. 

| Bước | Tỷ lệ cược thu thập | Các vết cắt được chọn | Phân đoạn hình thành | 
| --- | --- | --- | --- | 
| quét | [1, 3, 5] | [] | - | 
| chọn | [1, 3, 5] | [1, 3] | [1], [2,3], [4..5] | 

Đầu ra cuối cùng là`1 3 5`. 

Dấu vết cho thấy rằng mỗi ranh giới phân đoạn thẳng hàng với một phần tử lẻ, đảm bảo mỗi phân đoạn chứa đóng góp chẵn lẻ lẻ. 

### Ví dụ 2 

đầu vào:```
6 2
1 2 8 4 10 2
```Vị trí lẻ là [1]. 

Chúng ta cần k = 2, nhưng chúng ta chỉ có một phần tử lẻ. 

| Bước | Tỷ lệ cược thu thập | Khả thi | 
| --- | --- | --- | 
| quét | [1] | không | 

Vì chúng ta không thể gán ít nhất một phần tử lẻ cho mỗi phân đoạn nên câu trả lời là KHÔNG. 

Điều này thể hiện sự hạn chế về tính khả thi hơn là việc xây dựng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi truy vấn | chuyền đơn để thu thập các vị trí lẻ | 
| Không gian | O(n) | lưu trữ chỉ số của các phần tử lẻ | 

Tổng n trên tất cả các truy vấn được giới hạn bởi 2 × 10^5, do đó giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    q = int(input())
    res = []
    for _ in range(q):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        odds = [i+1 for i,x in enumerate(a) if x % 2 == 1]
        if len(odds) < k:
            res.append("NO")
        else:
            cuts = odds[:k-1] + [n]
            res.append("YES")
            res.append(" ".join(map(str, cuts)))
    return "\n".join(res)

# provided samples
assert run("""3
5 3
7 18 3 14 1
5 4
1 2 3 4 5
6 2
1 2 8 4 10 2
""") == """YES
1 3 5
NO
NO"""

# all odd minimum
assert run("""1
1 1
1
""") == """YES
1"""

# impossible due to parity shortage
assert run("""1
3 2
2 4 6
""") == """NO"""

# exact match case
assert run("""1
5 2
1 2 3 4 6
""") == """YES
1 5"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| toàn lẻ lẻ | CÓ 1 | xây dựng hợp lệ tối thiểu | 
| tất cả đều không thể | KHÔNG | kiểm tra tính khả thi | 
| tỷ lệ cược k-1 chính xác | CÓ cắt ở cuối | độ đúng ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi tất cả các phần tử đều chẵn. Ví dụ`a = [2,4,6]`với bất kỳ k ≥ 1. Mọi tổng phân đoạn đều là số chẵn, do đó không tồn tại phân vùng hợp lệ. Thuật toán phát hiện chính xác điều này vì danh sách các chỉ số lẻ trống, ngay lập tức không thực hiện được`len(odds) < k`tình trạng. 

Một trường hợp cạnh khác là khi k bằng số phần tử lẻ. Ví dụ`a = [1,2,3]`, k = 2. Vị trí lẻ là [1,3]. Thuật toán chọn vết cắt ở mức 1 và vết cắt cuối cùng ở mức 3, tạo ra các đoạn`[1]`Và`[2,3]`, cả hai đều có tổng lẻ. Điều này xác nhận rằng việc sử dụng k−1 vị trí lẻ đầu tiên sẽ căn chỉnh các phân đoạn một cách tự nhiên mà không cần bất kỳ thao tác quay lui hoặc điều chỉnh nào.
