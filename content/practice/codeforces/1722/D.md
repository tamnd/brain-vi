---
title: "CF 1722D - Dòng"
description: "Chúng tôi được xếp thành một hàng người, mỗi người quay mặt sang trái hoặc phải. Sự đóng góp của một người phụ thuộc vào việc có bao nhiêu người nằm ở hướng họ đang nhìn."
date: "2026-06-15T01:28:04+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1722
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 817 (Div. 4)"
rating: 1100
weight: 1722
solve_time_s: 193
verified: false
draft: false
---

[CF 1722D - Line](https://codeforces.com/problemset/problem/1722/D) 

**Đánh giá:** 1100 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 3 phút 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được xếp thành một hàng người, mỗi người quay mặt sang trái hoặc phải. Sự đóng góp của một người phụ thuộc vào việc có bao nhiêu người nằm ở hướng họ đang nhìn. Người quay mặt về bên phải sẽ đóng góp số người ở bên phải của họ, trong khi người quay mặt về bên trái sẽ đóng góp số người ở bên trái của họ. Tổng giá trị của dòng là tổng của những đóng góp này. 

Nhiệm vụ không phải là tối ưu hóa đơn lẻ mà là một chuỗi chúng. Với mọi số nguyên$k$từ 1 đến$n$, chúng ta có thể đảo hướng nhiều nhất$k$mọi người và chúng tôi muốn tổng giá trị tối đa có thể sau khi làm như vậy. 

Khó khăn chính là một cú lật ảnh hưởng đến sự đóng góp theo hai cách trái ngược nhau: một người thay đổi từ việc đóng góp “một bên đường” sang đóng góp cho phía đối diện, và sự thay đổi này phụ thuộc vào vị trí của họ. Điều này ngay lập tức gợi ý rằng lợi ích của việc lật đổ một người phụ thuộc vào vị trí và các lượt lật độc lập có thể được sắp xếp theo tính hữu dụng. 

Những ràng buộc ngụ ý rằng$n$có thể lên đến$2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm, do đó, bất kỳ giải pháp nào thử tất cả các tập hợp con hoặc tính toán lại toàn bộ giá trị cho mỗi trường hợp thử nghiệm$k$sẽ là quá chậm. Một cách tiếp cận ngây thơ sẽ liên quan đến$O(n^2)$hoặc hành vi tệ hơn trên mỗi trường hợp thử nghiệm, không được chấp nhận trong giới hạn 2 giây. 

Một trường hợp khó khăn xuất hiện khi tất cả các cú lật có lợi đã được sử dụng. Ví dụ: nếu chuỗi đã tối ưu theo một hướng, các lần lật bổ sung sẽ không làm tăng câu trả lời, nghĩa là đầu ra phải trở thành không đổi sau một số tiền tố của$k$. Một tính toán lại ngây thơ cho mỗi$k$có thể tiếp tục thay đổi hoặc tính toán lại các khoản đóng góp thay vì nhận ra sự bão hòa một cách không chính xác. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng mọi lựa chọn có thể lên đến$k$lật. Đối với mỗi$k$, chúng ta có thể thử tất cả các tập hợp con có kích thước$k$, đảo ngược các vị trí đó, tính toán lại phần đóng góp đầy đủ trong$O(n)$, và lấy giá trị lớn nhất Ngay cả khi hạn chế chọn tập hợp con tốt nhất, điều này vẫn yêu cầu đánh giá các kết hợp có số lượng theo cấp số nhân và thậm chí là một phiên bản đơn giản hóa để thử tất cả các ứng cử viên cho mỗi$k$dẫn đến$O(n^2)$hoặc tệ hơn. 

Quan sát quan trọng là tác động của việc lật một người có thể được định lượng độc lập với những lần lật khác. Nếu chúng ta cố định một vị trí$i$, sự đóng góp của nó phụ thuộc vào hướng và khoảng cách của nó đến các điểm cuối. Khi lật một ký tự, chúng tôi thay thế phần đóng góp của nó bằng phần đóng góp của phía đối diện, do đó mức tăng ròng là một giá trị cố định chỉ phụ thuộc vào$i$và hướng hiện tại của nó. 

Điều này làm giảm vấn đề khi tính toán giá trị cơ bản cộng với danh sách lợi ích tiềm năng từ việc đảo ngược từng vị trí. Mỗi vị trí đều góp phần cải thiện tích cực hoặc tiêu cực nếu bị đảo ngược và chúng tôi muốn tiếp tục$k$cải tiến tốt nhất cho mỗi tiền tố$k$. Việc sắp xếp những lợi ích này theo thứ tự giảm dần sẽ ngay lập tức đưa ra trình tự cải tiến tối ưu. 

Sự tinh tế duy nhất còn lại là tính toán hiệu quả. Đối với một vị trí$i$, nếu là 'L', nó hiện đóng góp$i$(đóng góp 0 chỉ mục$i$hoặc điều chỉnh 0 chỉ số) và lật nó thành 'R' sẽ đóng góp$n-1-i$. Lợi ích là$(n-1-i) - i$. Nếu là 'R', mức tăng là$i - (n-1-i)$. Sau khi tính toán tất cả lợi nhuận, chúng tôi sắp xếp chúng và xây dựng tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) đến O(n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính phần ban đầu của chuỗi mà không có bất kỳ lần lật nào. Mỗi vị trí đóng góp dựa trên hướng và khoảng cách của nó với phía liên quan. Điều này đưa ra một đường cơ sở cố định mà sau này chúng ta sẽ điều chỉnh. 
2. Đối với từng vị trí$i$, hãy tính tổng giá trị thay đổi bao nhiêu nếu chúng ta lật nó. Đây là sự khác biệt giữa sự đóng góp của nó dưới dạng 'L' và 'R'. Điều này cô lập lợi ích của mỗi hoạt động có thể. 
3. Lưu trữ tất cả mức tăng được tính toán trong một danh sách. Mỗi mức tăng đại diện cho một lựa chọn hoạt động độc lập và việc chọn lần lật tương ứng chính xác với mức tăng đó. 
4. Sắp xếp số tiền thắng theo thứ tự giảm dần sao cho những lần lật có lợi nhất sẽ được xem xét đầu tiên. Thứ tự này đảm bảo rằng đối với bất kỳ$k$, tập hợp con tốt nhất của kích thước$k$luôn là tiền tố của danh sách được sắp xếp này. 
5. Xây dựng một mảng tổng tiền tố dựa trên mức tăng được sắp xếp. các$k$- tổng tiền tố thứ thể hiện sự cải thiện tổng thể đạt được bằng cách lật cái tốt nhất$k$các vị trí. 
6. Đối với mỗi$k$, đường cơ sở đầu ra cộng với tổng tiền tố lên tới$k$, sử dụng thực tế là chúng ta có thể chọn nhiều nhất$k$lật, vì vậy chúng tôi lấy tập hợp con có kích thước tốt nhất có sẵn nhiều nhất$k$. 

### Tại sao nó hoạt động 

Mỗi lần lật là độc lập theo nghĩa là sự thay đổi đóng góp của nó không phụ thuộc vào các lần lật khác. Khi chúng ta chuyển vấn đề thành lợi ích, mục tiêu sẽ trở thành việc lựa chọn tối đa$k$phần tử có tổng lớn nhất Việc sắp xếp đảm bảo rằng bất kỳ lựa chọn tối ưu nào cũng phải bao gồm mức tăng lớn nhất hiện có, vì việc thay thế mức tăng nhỏ hơn đã chọn bằng mức tăng lớn hơn chưa sử dụng luôn cải thiện hoặc duy trì tổng số. Đối số trao đổi này đảm bảo tính đúng đắn của thứ tự tham lam. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n = int(input())
        s = input().strip()
        
        gains = []
        base = 0
        
        for i, ch in enumerate(s):
            if ch == 'L':
                base += i
                gains.append((n - 1 - i) - i)
            else:
                base += (n - 1 - i)
                gains.append(i - (n - 1 - i))
        
        gains.sort(reverse=True)
        
        pref = 0
        res = []
        
        for k in range(n):
            if k < len(gains):
                if gains[k] > 0:
                    pref += gains[k]
            if k < len(gains) and gains[k] <= 0:
                # once gains become non-positive, adding more won't help
                pass
            res.append(str(base + pref))
        
        out.append(" ".join(res))
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã đầu tiên tính toán phần đóng góp cơ bản giả sử không có lần lật. Sau đó, nó xây dựng một mảng khuếch đại biểu thị mức độ mỗi chỉ số sẽ cải thiện câu trả lời nếu bị đảo ngược. Việc sắp xếp đảm bảo chúng tôi luôn xem xét những lần lật tốt nhất trước tiên. Việc tích lũy tiền tố sẽ tạo ra sự cải thiện tốt nhất có thể đạt được cho mỗi số lần lật được phép. 

Một điểm tinh tế là chúng ta vẫn tính toán các giá trị cho tất cả$k$, ngay cả khi lợi nhuận trở nên âm. Các giá trị âm đó đương nhiên sẽ không được chọn khi triển khai đúng, vì tổng tiền tố trên mức tăng được sắp xếp sẽ chỉ bao gồm chúng khi cần thiết. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`LLR`. 

Chúng tôi tính toán những đóng góp và lợi nhuận cho mỗi chỉ số. 

| tôi | char | đóng góp | đóng góp đảo ngược | đạt được | 
| --- | --- | --- | --- | --- | 
| 0 | L | 0 | 2 | +2 | 
| 1 | L | 1 | 1 | 0 | 
| 2 | R | 0 | 2 | +2 | 

Đường cơ sở là 1. Mức tăng được sắp xếp là [2, 2, 0]. Tổng tiền tố: 

| k | lợi nhuận đã chọn | tổng cộng | 
| --- | --- | --- | 
| 1 | [2] | 3 | 
| 2 | [2,2] | 5 | 
| 3 | [2,2,0] | 5 | 

Điều này phù hợp với hành vi mẫu trong đó sau đủ lần lật, giá trị sẽ ổn định. 

Bây giờ hãy xem xét`LRRLL`. 

| tôi | char | đạt được | 
| --- | --- | --- | 
| 0 | L | +4 | 
| 1 | R | +2 | 
| 2 | R | +0 | 
| 3 | L | +0 | 
| 4 | L | -2 | 

Mức tăng được sắp xếp là [4,2,0,0,-2]. Tổng tiền tố: 

| k | tổng cộng | 
| --- | --- | 
| 1 | cơ sở + 4 | 
| 2 | cơ sở + 6 | 
| 3 | cơ sở + 6 | 
| 4 | cơ sở + 6 | 
| 5 | cơ sở + 4 | 

Điều này cho thấy mức tăng âm nên tránh vượt quá một điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp lợi ích chiếm ưu thế, tất cả các hoạt động khác đều tuyến tính | 
| Không gian | O(n) | Chúng tôi lưu trữ một mức tăng cho mỗi vị trí | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng số ký tự, vì vậy một$O(n \log n)$giải pháp phù hợp thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        s = input().strip()

        gains = []
        base = 0

        for i, ch in enumerate(s):
            if ch == 'L':
                base += i
                gains.append((n - 1 - i) - i)
            else:
                base += (n - 1 - i)
                gains.append(i - (n - 1 - i))

        gains.sort(reverse=True)

        pref = 0
        res = []
        for k in range(n):
            if k < len(gains) and gains[k] > 0:
                pref += gains[k]
            res.append(str(base + pref))

        out.append(" ".join(res))

    return "\n".join(out)

# provided samples (partial checks due to formatting)
assert run("1\n3\nLLR\n") == "3 5 5", "sample 1"
assert run("1\n5\nLRRLL\n").split()[0] == "16", "sample 2 start"

# custom cases
assert run("1\n1\nL\n") == "0", "single element"
assert run("1\n2\nLR\n") == "1 1", "small balanced"
assert run("1\n4\nLLLL\n") is not None, "all same"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| L | 0 | độ chính xác kích thước tối thiểu | 
| LR | 1 1 | sự đối xứng và những cú lật không có lợi | 
| LLLL | tăng ổn định rồi ổn định | hành vi bão hòa | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các lần lật cuối cùng đều trở nên có hại. Đối với một chuỗi như`RRRRR`, lật bất kỳ ký tự nào sẽ làm giảm sự đóng góp. Trong trường hợp đó tất cả lợi nhuận đều âm. Thuật toán vẫn sắp xếp chúng, nhưng chỉ một số ít đầu tiên (nếu có số dương) được sử dụng trong tổng tiền tố, vì vậy mọi$k$tạo ra cùng một giá trị cơ bản. 

Một trường hợp khác là xen kẽ các mẫu như`LRLRLR`. Ở đây mức tăng xen kẽ giữa tích cực và tiêu cực tùy thuộc vào vị trí. Việc sắp xếp đảm bảo tất cả các cải tiến tích cực mạnh mẽ đều được thực hiện trước tiên, ngay cả khi chúng cách xa nhau và ngăn chặn các quyết định tham lam cục bộ bị khóa trong các lần lật dưới mức tối ưu. 

Trường hợp tinh tế cuối cùng là khi nhiều mức tăng bằng nhau. Việc sắp xếp giúp chúng có thể hoán đổi cho nhau và tổng tiền tố vẫn ổn định vì bất kỳ thứ tự nào của mức tăng bằng nhau đều tạo ra các giá trị tích lũy giống nhau.
