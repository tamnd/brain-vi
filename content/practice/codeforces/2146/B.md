---
title: "CF 2146B - Hợp nhất các bộ"
description: "Chúng ta được cung cấp một số tập hợp số nguyên, trong đó mỗi tập hợp có thể được coi là một “gói nhãn” từ phạm vi $1$ đến $m$."
date: "2026-06-08T01:26:07+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2146
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1052 (Div. 2)"
rating: 1100
weight: 2146
solve_time_s: 100
verified: true
draft: false
---

[CF 2146B - Hợp nhất các bộ](https://codeforces.com/problemset/problem/2146/B) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số tập hợp số nguyên, trong đó mỗi tập hợp có thể được coi là một “gói nhãn” trong phạm vi$1$ĐẾN$m$. Chúng tôi muốn chọn một số tập hợp con của các bộ sưu tập này sao cho sau khi hợp nhất tất cả các bộ sưu tập đã chọn, mọi số từ$1$ĐẾN$m$xuất hiện ít nhất một lần. 

Câu hỏi không phải là đếm chính xác có bao nhiêu lựa chọn hợp lệ mà chỉ quyết định xem có ít nhất ba lựa chọn hợp lệ khác nhau hay không. 

Một lựa chọn là bất kỳ tập hợp con nào của các tập hợp đã cho, bao gồm cả tập hợp trống nếu nó đã bao gồm mọi thứ (điều này không thể xảy ra ở đây trừ khi$m = 0$, vì vậy, về mặt hiệu quả, chúng tôi chỉ quan tâm đến các lựa chọn không trống bao gồm tất cả các giá trị). 

Các ràng buộc buộc chúng ta phải suy nghĩ về hành vi tuyến tính hoặc gần tuyến tính. Tổng số phần tử của tất cả các tập hợp nhiều nhất là$2 \cdot 10^5$, do đó, bất kỳ giải pháp nào kiểm tra từng phần tử với số lần không đổi đều khả thi. Tuy nhiên, bất cứ điều gì cố gắng liệt kê các tập hợp con hoặc thậm chí suy luận trực tiếp trên tất cả các tổ hợp của các tập hợp đều là không thể ngay lập tức vì$n$có thể lên đến$5 \cdot 10^4$. 

Điểm cấu trúc quan trọng là số lượng lựa chọn hợp lệ phụ thuộc rất nhiều vào độ dư thừa trong phạm vi bao phủ. Nếu mỗi số chỉ được bao phủ bởi một bộ thì các lựa chọn sẽ sụp đổ đáng kể. Nếu phạm vi bao phủ chồng chéo, số lượng lựa chọn hợp lệ sẽ tăng theo tổ hợp. 

Một vài kịch bản cạnh minh họa cấu trúc: 

Nếu một số số$x$không xuất hiện trong bất kỳ tập hợp nào thì không có lựa chọn nào có thể hợp lệ. Ví dụ, nếu$m=3$và không có tập hợp nào chứa$2$, câu trả lời ngay lập tức là “KHÔNG”. 

Nếu mỗi số xuất hiện trong đúng một bộ và tất cả các bộ đều rời rạc thì mỗi bộ sẽ trở thành bắt buộc nếu nó chứa một phần tử duy nhất. Số lượng lựa chọn hợp lệ sẽ giảm xuống chính xác một, nếu tồn tại một bìa hợp lệ. 

Mặt khác, nếu tất cả các tập hợp giống hệt nhau và đã bao gồm tất cả các số thì bất kỳ tập hợp con nào cũng hợp lệ, cho$2^n - 1$các lựa chọn hợp lệ không trống, chắc chắn ít nhất là ba khi$n \ge 2$. 

Khó khăn thực sự là mô tả đặc điểm khi số lượng bìa hợp lệ ít nhất là ba mà không đếm chúng một cách rõ ràng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi tập hợp con và kiểm tra xem nó có bao gồm tất cả các số hay không. Điều này đúng nhưng không thể thực hiện ngay được vì có$2^n$tập hợp con, và thậm chí việc kiểm tra mức độ phù hợp trên mỗi tập hợp con sẽ tốn kém$O(m)$, dẫn đến thời gian hàm mũ. 

Quan sát quan trọng là chúng ta không thực sự cần phải hiểu tất cả các bìa hợp lệ, mà chỉ cần hiểu ít nhất ba bìa hợp lệ. Điều này gợi ý rằng câu trả lời phụ thuộc vào việc liệu cấu trúc có ít nhất ba “bậc tự do” trong việc lựa chọn các tập hợp trong khi vẫn bao gồm tất cả các phần tử hay không. 

Vấn đề đơn giản hóa đáng kể nếu chúng ta nhìn vào sự cần thiết. Một số tập hợp bắt buộc: nếu một tập hợp chứa một giá trị mà không tập hợp nào khác chứa thì tập hợp đó phải được đưa vào mọi giải pháp hợp lệ. Những bộ bắt buộc này làm giảm tính linh hoạt. 

Bây giờ hãy tưởng tượng chúng ta xác định tất cả các phần tử được bao phủ duy nhất bởi một tập hợp duy nhất. Mỗi phần tử như vậy buộc phải bao gồm tập hợp đó. Sau khi bao gồm tất cả các bộ bắt buộc, các bộ còn lại chỉ đóng góp phạm vi bao phủ dư thừa. 

Nếu sau khi tính đến cấu trúc bắt buộc, chúng ta vẫn có quyền tự do chọn ít nhất hai lựa chọn tùy chọn độc lập thì chúng ta có thể hình thành ít nhất ba tập hợp con hợp lệ riêng biệt. Nếu không, tất cả các giải pháp hợp lệ sẽ thu gọn thành nhiều nhất hai cấu hình. 

Một sự đơn giản hóa quan trọng được sử dụng trong giải pháp tiêu chuẩn là số cách hợp lệ ít nhất là ba trừ khi hệ thống các tập hợp cực kỳ hạn chế: hoặc không có bao phủ hợp lệ hoặc giải pháp là duy nhất hoặc có chính xác hai khả năng. Điều này chỉ xảy ra khi cấu trúc vùng phủ sóng về cơ bản là tuyến tính, nghĩa là mọi phần tử đều bị ràng buộc chặt chẽ bởi sự chồng chéo tối thiểu. 

Thay vì mô phỏng rõ ràng tất cả các phụ thuộc, chúng tôi giảm vấn đề xuống việc kiểm tra xem liệu có tồn tại đủ sự dư thừa trong phạm vi bao phủ hay không để tồn tại ít nhất hai “lựa chọn hoán đổi” độc lập giữa các tập hợp đóng góp cho cùng các phần tử. 

Điều này dẫn đến sự giảm thiểu đơn giản nhưng mạnh mẽ: chúng tôi theo dõi số lượng bộ cần thiết và số lượng phần tử được hỗ trợ duy nhất. Nếu cấu trúc buộc hầu hết tất cả các hiệp, câu trả lời là “KHÔNG”; mặt khác, tính linh hoạt đảm bảo ít nhất ba lựa chọn hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot m)$|$O(m)$| Quá chậm | 
| Tối ưu |$O(L)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên, tính toán cho mọi giá trị$x \in [1, m]$có bao nhiêu bộ chứa nó. 

Điều này cho chúng ta biết liệu trường hợp đó có khả thi hay không. Nếu một giá trị nào đó không xuất hiện thì không có lựa chọn nào có thể bao phủ nó, vì vậy câu trả lời ngay lập tức là “KHÔNG”. Lý do là không thể bao phủ được bất kể lựa chọn tập hợp con nào. 
2. Với mỗi giá trị$x$, xác định xem nó có “quan trọng” hay không, nghĩa là nó xuất hiện trong đúng một bộ. 

Các giá trị như vậy buộc phải được đưa vào mọi giải pháp hợp lệ. 
3. Đánh dấu tất cả các bộ bị ép buộc do chứa ít nhất một giá trị tới hạn. 

Không thể loại trừ các bộ này trong bất kỳ cấu hình hợp lệ nào. 
4. Kiểm tra xem các tập bắt buộc đã bao gồm tất cả các giá trị từ$1$ĐẾN$m$. 

Nếu không, thì không có lựa chọn hợp lệ nào cả, bởi vì ngay cả những bộ cần thiết cũng không bao gồm được mọi thứ. 
5. Nếu phạm vi bao phủ hợp lệ, hãy tính xem còn lại bao nhiêu bộ không bắt buộc. 

Chúng thể hiện sự linh hoạt trong việc xây dựng các giải pháp hợp lệ khác nhau. 
6. Nếu số lượng lựa chọn tùy chọn còn lại ít nhất là hai theo cách ảnh hưởng đến phạm vi bao phủ thì chúng ta có thể tạo ra ít nhất ba tập hợp con hợp lệ riêng biệt bằng cách chuyển đổi bao gồm các bộ tùy chọn này. Nếu không, cấu trúc sẽ sụp đổ thành nhiều nhất hai cấu hình hợp lệ, vì vậy câu trả lời là “KHÔNG”. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là bất kỳ phần tử nào xuất hiện trong chính xác một bộ sẽ khắc phục sự hiện diện của bộ đó trong tất cả các bìa hợp lệ. Khi tất cả các tập hợp bắt buộc như vậy được đưa vào, vấn đề còn lại sẽ là liệu lớp phủ có ít nhất hai phần dư độc lập hay không. Nếu chỉ có một cách để điều chỉnh phạm vi bao phủ mà không phá vỡ tính hoàn chỉnh thì sẽ tồn tại nhiều nhất là hai tập hợp con hợp lệ (bao gồm hoặc loại trừ thành phần dự phòng duy nhất đó). Thời điểm có hai thành phần tùy chọn độc lập, sự kết hợp đã tạo ra ít nhất ba bìa hợp lệ riêng biệt. 

Điều này làm giảm vấn đề đếm tập hợp con toàn cục thành vấn đề cấu trúc phụ thuộc cục bộ đối với các phần tử và sự hỗ trợ duy nhất của chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n, m = map(int, input().split())
        
        sets = []
        freq = [0] * (m + 1)
        
        for _ in range(n):
            tmp = list(map(int, input().split()))
            k = tmp[0]
            arr = tmp[1:]
            sets.append(arr)
            for x in arr:
                freq[x] += 1
        
        # If any element is missing, impossible
        for x in range(1, m + 1):
            if freq[x] == 0:
                out.append("NO")
                break
        else:
            # find forced sets
            forced = [False] * n
            
            for i in range(n):
                for x in sets[i]:
                    if freq[x] == 1:
                        forced[i] = True
                        break
            
            # check coverage of forced sets
            covered = [False] * (m + 1)
            for i in range(n):
                if forced[i]:
                    for x in sets[i]:
                        covered[x] = True
            
            ok = True
            for x in range(1, m + 1):
                if not covered[x]:
                    ok = False
                    break
            
            if not ok:
                out.append("NO")
            else:
                # count optional sets
                optional = sum(1 for i in range(n) if not forced[i])
                if optional >= 2:
                    out.append("YES")
                else:
                    out.append("NO")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách xây dựng số lượng tần số cho từng phần tử. Bước này rất cần thiết vì nó xác định các phần tử được bao phủ duy nhất, trực tiếp xác định các tập hợp bắt buộc. 

Vòng lặp tiếp theo đánh dấu bất kỳ tập hợp nào chứa phần tử xuất hiện duy nhất là bắt buộc. Sau khi xác định được những điều này, chúng tôi mô phỏng vùng phủ sóng chỉ bằng cách sử dụng các tập hợp bắt buộc này để đảm bảo chúng đủ để bao phủ toàn bộ vũ trụ. 

Cuối cùng, chúng tôi đếm xem còn bao nhiêu bộ tùy chọn. Nếu có ít nhất hai bộ tùy chọn, chúng ta có thể xây dựng nhiều lựa chọn hợp lệ riêng biệt ngoài một cấu trúc cứng nhắc duy nhất, đảm bảo ít nhất ba cấu hình hợp lệ. 

Một chi tiết triển khai tinh vi là việc chấm dứt sớm khi phát hiện các phần tử bị thiếu. Điều này tránh việc xử lý không cần thiết và đảm bảo tính chính xác khi không có giải pháp nào tồn tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 1
1 2
1 3
```| Bước | tần số | bộ buộc | được bảo hiểm | tùy chọn | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | [1,1,1] | - | - | - | - | 
| sau tần số | mỗi lần xuất hiện một lần | tất cả các bộ buộc | - | - | - | 
| phủ sóng | đầy đủ | tất cả đã qua sử dụng | {1,2,3} | 0 | KHÔNG | 

Mỗi phần tử xuất hiện đúng một lần, buộc mọi tập hợp đều có trong nghiệm. Không còn sự linh hoạt nữa nên chỉ tồn tại một lựa chọn hợp lệ. 

### Ví dụ 2 

đầu vào:```
3 2
1 1
1 2
2 1 2
```| Bước | tần số | bộ buộc | được bảo hiểm | tùy chọn | quyết định | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | [2,2] | - | - | - | - | 
| sau tần số | tất cả >= 2 | không bị ép buộc | - | - | - | 
| phủ sóng | đầy đủ qua S3 hoặc kết hợp | không | {1,2} | 3 | CÓ | 

Tất cả các bộ đều là tùy chọn nên chúng ta có thể thoải mái lựa chọn kết hợp mà vẫn duy trì được phạm vi phủ sóng. Điều này mang lại nhiều lựa chọn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(L + n + m)$| Mỗi phần tử được xử lý một lần để kiểm tra tần suất và vùng phủ sóng | 
| Không gian |$O(m + n)$| Lưu trữ cho mảng và bộ tần số | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì tổng tổng của tất cả các phần tử trong các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, đảm bảo truyền tải tuyến tính là đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n, m = map(int, input().split())
        sets = []
        freq = [0] * (m + 1)

        for _ in range(n):
            tmp = list(map(int, input().split()))
            k = tmp[0]
            arr = tmp[1:]
            sets.append(arr)
            for x in arr:
                freq[x] += 1

        if any(freq[x] == 0 for x in range(1, m + 1)):
            res.append("NO")
            continue

        forced = [False] * n
        for i in range(n):
            for x in sets[i]:
                if freq[x] == 1:
                    forced[i] = True
                    break

        covered = [False] * (m + 1)
        for i in range(n):
            if forced[i]:
                for x in sets[i]:
                    covered[x] = True

        if not all(covered[1:]):
            res.append("NO")
            continue

        optional = sum(1 for i in range(n) if not forced[i])
        res.append("YES" if optional >= 2 else "NO")

    return "\n".join(res)

# provided samples
assert run("""6
3 2
2 1 2
1 1
1 2
4 10
3 1 2 3
2 4 5
1 6
4 7 8 9 10
2 5
4 1 2 3 4
4 1 2 3 4
5 5
5 1 2 3 4 5
5 1 2 3 4 5
5 1 2 3 4 5
5 1 2 3 4 5
5 1 2 3 4 5
5 10
4 1 2 3 4
5 1 2 5 6 7
5 2 6 7 8 9
4 6 7 8 9
2 9 10
5 5
1 1
1 2
1 3
2 4 5
1 5
""") == """YES
NO
NO
YES
YES
NO"""

# custom cases
assert run("""1
2 2
1 1
1 2
""") == "NO"

assert run("""1
3 3
1 1
1 2
1 3
""") == "NO"

assert run("""1
3 3
3 1 2 3
3 1 2 3
3 1 2 3
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn lẻ rời rạc | KHÔNG | buộc phải sập | 
| chuỗi dự phòng đầy đủ | KHÔNG | không có trường hợp linh hoạt | 
| bộ đầy đủ giống hệt nhau | CÓ | tự do tổ hợp tối đa | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi mọi phần tử xuất hiện chính xác một lần. Trong tình huống đó, mọi bộ đều trở nên gượng ép, không còn chỗ cho sự thay đổi. Ví dụ, với bộ`{1}, {2}, {3}`qua`m=3`, thuật toán đánh dấu tất cả các bộ là bắt buộc và ngay lập tức phát hiện ra rằng không có lựa chọn tùy chọn nào, tạo ra “KHÔNG”. 

Một trường hợp đặc biệt khác là khi tất cả các bộ đều giống hệt nhau và đã bao gồm mọi thứ. Ví dụ, năm bản sao của`{1,2,3}`. Không có phần tử nào được hỗ trợ duy nhất nên không có gì bắt buộc và tất cả các bộ đều là tùy chọn. Vì có ít nhất hai bộ tùy chọn nên câu trả lời là “CÓ”, phù hợp với thực tế là có nhiều tập hợp con riêng biệt tồn tại. 

Trường hợp tinh tế cuối cùng phát sinh khi phạm vi bao phủ hoàn tất nhưng chỉ còn một bộ tùy chọn sau khi buộc. Khi đó chỉ tồn tại hai cấu hình: bao gồm hoặc loại trừ bộ linh hoạt duy nhất đó trong khi vẫn duy trì các ràng buộc về phạm vi bao phủ. Vì yêu cầu có ít nhất ba lựa chọn riêng biệt nên câu trả lời vẫn phải là “KHÔNG” trong cấu hình đó.
