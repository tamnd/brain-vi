---
title: "CF 104487D - Tính tương tự"
description: "Chúng ta có một số chuỗi, tất cả đều có cùng độ dài cố định. Chúng tôi được phép xây dựng một chuỗi mới có cùng độ dài. Mục tiêu là chọn chuỗi được xây dựng này sao cho nó khớp với tổng số các chuỗi đã cho nhiều nhất có thể."
date: "2026-06-30T12:38:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104487
codeforces_index: "D"
codeforces_contest_name: "Tishreen + SVU CPC 2023"
rating: 0
weight: 104487
solve_time_s: 41
verified: true
draft: false
---

[CF 104487D - Tính tương tự](https://codeforces.com/problemset/problem/104487/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số chuỗi, tất cả đều có cùng độ dài cố định. Chúng tôi được phép xây dựng một chuỗi mới có cùng độ dài. Mục tiêu là chọn chuỗi được xây dựng này sao cho nó khớp với tổng số các chuỗi đã cho nhiều nhất có thể. 

Một trận đấu được tính theo từng vị trí. Đối với một vị trí cố định, nếu chuỗi được xây dựng của chúng tôi có cùng ký tự với một chuỗi nhất định tại vị trí đó, chúng tôi sẽ đạt được một đơn vị tương tự cho chuỗi đó. Điểm cuối cùng là tổng của các trận đấu này trên tất cả các chuỗi và tất cả các vị trí. 

Điều chỉnh lại điều này theo cách có cấu trúc hơn, mỗi vị trí trong chuỗi cuối cùng đóng góp độc lập vào tổng điểm. Tại vị trí i, chúng ta chọn một ký tự đơn và lựa chọn đó đồng thời ảnh hưởng đến số lượng trong số n chuỗi khớp với vị trí đó. 

Các ràng buộc là nhỏ, với cả số lượng chuỗi và độ dài của chúng bị giới hạn bởi 50. Điều này ngay lập tức ngụ ý rằng ngay cả một giải pháp bậc ba hoặc bậc hai cho mỗi trường hợp thử nghiệm cũng dễ dàng đủ nhanh. Quét toàn bộ tất cả các ký tự ở tất cả các vị trí tối đa là 2500 ô cho mỗi trường hợp thử nghiệm và thậm chí với T lên tới 50, tổng công việc là không đáng kể. 

Không có trường hợp phức tạp nào liên quan đến thứ tự hoặc sự phụ thuộc giữa các vị trí, vì mỗi vị trí đóng góp độc lập. Vấn đề tinh tế duy nhất có thể cản trở cách tiếp cận ngây thơ là cố gắng xây dựng chuỗi một cách tham lam mà không nhận ra rằng mỗi vị trí được giải quyết một cách độc lập và tối ưu bằng cách tối đa hóa tần số cục bộ. 

Ví dụ: nếu tại một vị trí, chúng tôi chọn một ký tự xuất hiện 2 lần thay vì một ký tự xuất hiện 3 lần, thì quyết định đó không thể được đền bù ở nơi khác vì đóng góp không chuyển giữa các vị trí. Một cạm bẫy khác là cố gắng xây dựng chuỗi thực tế một cách không cần thiết khi chỉ yêu cầu điểm số, điều này có thể dẫn đến sự phức tạp hoặc sai sót có thể tránh được. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi chuỗi có độ dài m có thể có trên bảng chữ cái được ngụ ý bởi đầu vào. Với mỗi chuỗi ứng cử viên, hãy tính độ tương tự của nó với tất cả n chuỗi đã cho bằng cách so sánh tất cả m vị trí. Chi phí để đánh giá một ứng cử viên là O(nm) và số lượng ứng viên là cấp số nhân tính bằng m. Ngay cả khi hạn chế các ký tự xuất hiện trong đầu vào, hệ số phân nhánh vẫn lớn, khiến phương pháp này không khả thi vượt quá m nhỏ. 

Quan sát chính là không có sự tương tác giữa các vị trí. Sự đóng góp của vị trí i chỉ phụ thuộc vào nhân vật chúng ta chọn ở vị trí i và độc lập với tất cả các vị trí khác. Điều này tách vấn đề thành m bài toán con riêng biệt, mỗi bài toán con một cột. 

Đối với vị trí cố định i, chúng ta chỉ cần biết có bao nhiêu chuỗi chứa mỗi ký tự ở vị trí đó. Nếu ký tự c xuất hiện tần số [c] lần trong n chuỗi ở vị trí i thì việc chọn c sẽ đóng góp chính xác tần số [c] từ vị trí đó. Do đó, lựa chọn tối ưu chỉ đơn giản là ký tự xuất hiện thường xuyên nhất trong cột đó. 

Vì vậy, thay vì tìm kiếm trên toàn bộ chuỗi, chúng tôi tính tần số trên mỗi cột và tính tổng tần số tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O( | Σ | ^m · n m) | 
| Tối ưu | O(n m) | O( | Σ | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đọc n và m, sau đó đọc tất cả các chuỗi. 
2. Với mỗi vị trí i từ 0 đến m−1, hãy tính sơ đồ tần số của các ký tự xuất hiện trong cột đó trên tất cả n chuỗi. 
3. Đối với mỗi cột, hãy tìm tần suất tối đa trong số tất cả các ký tự xuất hiện ở đó. 
4. Thêm mức tối đa này vào câu trả lời. 
5. Xuất tổng tích lũy cho test case. 

Lựa chọn thiết kế chính là xử lý từng cột một cách độc lập. Điều này hợp lệ vì việc chọn một ký tự ở vị trí i chỉ ảnh hưởng đến các kết quả khớp ở vị trí i và không ảnh hưởng đến bất kỳ vị trí nào khác. 

### Tại sao nó hoạt động

Tổng số điểm có thể được viết dưới dạng tổng của các vị trí, trong đó mỗi thuật ngữ chỉ phụ thuộc vào ký tự được chọn ở vị trí đó. Vì không có sự kết hợp giữa các vị trí nên việc tối ưu hóa từng số hạng một cách độc lập sẽ mang lại giải pháp tối ưu toàn cục. Đối với mỗi cột, bất kỳ sai lệch nào so với ký tự thường xuyên nhất sẽ làm giảm nghiêm trọng sự đóng góp của cột đó mà không cải thiện bất kỳ cột nào khác, do đó không tồn tại sự cân bằng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        arr = [input().strip() for _ in range(n)]
        
        ans = 0
        
        for j in range(m):
            freq = {}
            for i in range(n):
                c = arr[i][j]
                freq[c] = freq.get(c, 0) + 1
            ans += max(freq.values())
        
        out.append(str(ans))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp lặp lại từng cột, xây dựng từ điển tần số cho từng cột. Vòng lặp bên trong trên n chuỗi là không thể tránh khỏi vì chúng ta phải kiểm tra từng ký tự ít nhất một lần. Từ điển được đặt lại trên mỗi cột, đảm bảo không có ô nhiễm giữa các cột. 

Một lỗi phổ biến là cố gắng duy trì tần số chung trên toàn bộ ma trận, điều này sẽ trộn lẫn các vị trí không liên quan và phá hủy tính chính xác. Một vấn đề nhỏ khác là quên đặt lại bản đồ tần số cho mỗi cột, điều này sẽ tích lũy số lượng trên các cột một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
abc
ebd
fbd
```Chúng tôi tính toán tần số theo cột. 

| Cột | Tần số | Ký tự tối đa | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | a:1, e:1, f:1 | bất kỳ (1) | 1 | 
| 1 | b:3 | b | 3 | 
| 2 | c:1, d:2 | d | 2 | 

Tổng cộng = 1 + 3 + 2 = 6. 

Điều này cho thấy rằng mặc dù chuỗi được xây dựng tối ưu là "ebd", nhưng chúng ta không bao giờ cần phải xây dựng nó một cách rõ ràng; chỉ có số lượng cột quan trọng. 

### Ví dụ 2 

đầu vào:```
2 3
abc
aff
```| Cột | Tần số | Ký tự tối đa | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | một:2 | một | 2 | 
| 1 | b:1, f:1 | b hoặc f | 1 | 
| 2 | c:1, f:1 | c hoặc f | 1 | 

Tổng cộng = 2 + 1 + 1 = 4. 

Ví dụ này chứng minh rằng mối quan hệ về tần số không ảnh hưởng đến câu trả lời, vì chỉ có số đếm mới quan trọng chứ không phải ký tự nào được chọn trong số cực đại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nmT) | Mỗi trường hợp kiểm thử quét tất cả n chuỗi trên m cột | 
| Không gian | O(1) mỗi cột | Mỗi lần chỉ có một bản đồ tần số nhỏ được lưu trữ | 

Với n, m 50 và T 50, tổng số thao tác tối đa là 125000 lần kiểm tra ký tự, nằm trong giới hạn tầm thường trong 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return "\n".join(solve() or [])

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        arr = [input().strip() for _ in range(n)]
        ans = 0
        for j in range(m):
            freq = {}
            for i in range(n):
                c = arr[i][j]
                freq[c] = freq.get(c, 0) + 1
            ans += max(freq.values())
        out.append(str(ans))
    return out

# provided sample
assert run("""1
3 3
abc
ebd
fbd
""") == "6"

# all equal strings
assert run("""1
3 3
aaa
aaa
aaa
""") == "9"

# binary tie case
assert run("""1
2 4
abab
baba
""") == "4"

# single string
assert run("""1
1 5
abcde
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các chuỗi bằng nhau | 9 | chồng chéo hoàn toàn tối đa | 
| xen kẽ nhị phân | 4 | xử lý buộc mỗi cột | 
| chuỗi đơn | 5 | tính đúng đắn của trường hợp cơ sở | 

## Vỏ cạnh 

Trường hợp tối thiểu có một chuỗi cho thấy mỗi cột đóng góp chính xác 1, vì chuỗi đó xác định kết quả khớp duy nhất có thể. Thuật toán xử lý việc này vì mỗi bản đồ tần số cột có một mục nhập duy nhất bằng n = 1, do đó giá trị tối đa luôn là 1. 

Một cột nặng, chẳng hạn như hai chuỗi có các ký tự khác nhau, vẫn cho kết quả chính xác vì tần số tối đa vẫn bằng 1. Thuật toán không phụ thuộc vào ký tự nào được chọn mà chỉ phụ thuộc vào số lần nó xuất hiện. 

Một ma trận hoàn toàn đồng nhất tạo ra điểm tối đa n × m. Bản đồ tần số trên mỗi cột luôn có một ký tự nổi trội duy nhất với số lượng n, do đó, tổng các cột mang lại kết quả tối đa dự kiến ​​mà không cần viết hoa đặc biệt.
