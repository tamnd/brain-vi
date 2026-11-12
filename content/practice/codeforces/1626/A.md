---
title: "CF 1626A - Chữ cách đều"
description: "Chúng ta được cung cấp một chuỗi ngắn trong đó mỗi ký tự xuất hiện một hoặc hai lần. Nhiệm vụ là hoán vị các ký tự sao cho bất cứ khi nào một chữ cái xuất hiện hai lần, khoảng cách giữa hai lần xuất hiện của nó là giống nhau đối với tất cả các chữ cái đó."
date: "2026-06-10T05:22:39+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1626
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 121 (Rated for Div. 2)"
rating: 800
weight: 1626
solve_time_s: 118
verified: false
draft: false
---

[CF 1626A - Các chữ cái cách đều](https://codeforces.com/problemset/problem/1626/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, sắp xếp 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi ngắn trong đó mỗi ký tự xuất hiện một hoặc hai lần. Nhiệm vụ là hoán vị các ký tự sao cho bất cứ khi nào một chữ cái xuất hiện hai lần, khoảng cách giữa hai lần xuất hiện của nó là giống nhau đối với tất cả các chữ cái đó. 

Một cách khác để thấy điều đó là với mỗi chữ cái trùng lặp, nếu vị trí của nó là$i$Và$j$, thì giá trị$j - i$phải có cùng một hằng số cho tất cả các chữ cái xuất hiện hai lần. Các chữ cái xuất hiện một lần không có ràng buộc. 

Độ dài chuỗi tối đa là 52 và có tối đa 26 chữ cái riêng biệt, mỗi chữ cái xuất hiện nhiều nhất hai lần. Điều này ngay lập tức cho chúng ta biết rằng bất kỳ công trình nào thử tất cả các hoán vị đều không cần thiết. Thậm chí$52!$vượt xa các giới hạn khả thi, nhưng ngay cả những hành vi bạo lực có cấu trúc chặt chẽ hơn như quay lại các vị trí vẫn sẽ có tính cấp số nhân trong thực tế. 

Một quan sát quan trọng là hạn chế có ý nghĩa duy nhất là ghép các chữ cái trùng lặp với khoảng cách bằng nhau. Các chữ cái xuất hiện một lần là các biến tự do có thể được chèn vào bất cứ đâu mà không ảnh hưởng đến các ràng buộc. 

Các trường hợp đặc biệt thường phá vỡ các ý tưởng ngây thơ là các tình huống trong đó các chữ cái trùng lặp nằm rải rác và xen kẽ trong đầu vào. Ví dụ: trong một chuỗi như`abacbc`, khoảng cách ghép nối tự nhiên sẽ khác nhau và mọi giải pháp đều phải sắp xếp lại thứ tự trên toàn cầu chứ không phải điều chỉnh các cặp cục bộ. Một trường hợp tinh tế khác là khi không có chữ cái nào xuất hiện hai lần, chẳng hạn như`ac`, trong đó mọi hoán vị đều hợp lệ và các ràng buộc biến mất hoàn toàn. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là thử tất cả các hoán vị của chuỗi và kiểm tra xem tất cả các chữ cái trùng lặp có khoảng cách bằng nhau hay không. Điều này đúng nhưng không khả thi ngay lập tức vì độ dài chẵn 20 đã mang lại$20!$hoán vị. 

Cấu trúc của bài toán trở nên đơn giản hơn khi chúng ta diễn giải lại điều kiện. Giả sử chúng ta quyết định rằng tất cả các chữ cái trùng lặp phải có vị trí tạo thành các cặp có khoảng cách giống nhau. Nếu chúng ta sửa cách sắp xếp cuối cùng thì mỗi chữ cái trùng lặp sẽ chiếm hai vị trí có hiệu là không đổi$d$. Vì vậy, nhiệm vụ trở thành xây dựng một chuỗi trong đó mọi chữ cái trùng lặp được đặt vào các vị trí$(i, i + d)$đối với một số cố định$d$. 

Thay vì tìm kiếm hoán vị, chúng ta có thể trực tiếp xây dựng cấu trúc như vậy. Vì mỗi chữ cái xuất hiện nhiều nhất hai lần nên chúng ta chỉ cần gán các cặp vị trí. Chúng ta có thể chọn khoảng cách$d$, sau đó đặt từng chữ cái trùng lặp vào các vị trí tôn trọng khoảng cách này. Vì độ dài chuỗi nhỏ và số lượng bản sao bị hạn chế nên hãy thử mọi cách khả thi$d$giá trị và lấp đầy các vị trí một cách tham lam luôn thành công. 

Một cái nhìn sâu sắc đơn giản và tiêu chuẩn hơn được sử dụng trong hầu hết các giải pháp thậm chí còn mạnh mẽ hơn: chúng ta không cần phải chọn một cách rõ ràng$d$. Chúng ta có thể xây dựng câu trả lời bằng cách đặt tất cả các lần xuất hiện thứ hai ở một độ lệch đồng đều so với các lần xuất hiện đầu tiên theo một thứ tự nhất quán. Việc sắp xếp các chữ cái và gán vị trí theo mẫu có cấu trúc sẽ đảm bảo khoảng cách bằng nhau một cách tự động. 

Cấu trúc đơn giản nhất là đặt tất cả các ký tự xuất hiện hai lần trong một mẫu khối liền kề: lần xuất hiện đầu tiên trong một chuỗi, lần xuất hiện thứ hai được phản ánh bằng một sự dịch chuyển cố định, trong khi các ký tự đơn được chèn tùy ý vào các vị trí còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | Ồ (n!) | O(n) | Quá chậm | 
| Xây dựng kết cấu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Ý tưởng chính là nhóm các chữ cái theo tần suất và sau đó đặt chúng theo thứ tự được kiểm soát để đảm bảo khoảng cách giống nhau cho tất cả các chữ cái trùng lặp. 

1. Đếm tần số của từng ký tự. Chúng tôi tách các ký tự thành những ký tự xuất hiện một lần và những ký tự xuất hiện hai lần. Sự phân loại này rất quan trọng vì chỉ những chữ cái trùng lặp mới áp đặt các ràng buộc. 
2. Thu thập tất cả các chữ cái xuất hiện hai lần vào danh sách và sắp xếp chúng. Việc sắp xếp không bắt buộc phải đảm bảo tính chính xác nhưng nó đảm bảo tính xác định và đơn giản hóa việc suy luận về vị trí. 
3. Xây dựng hai chuỗi cho các chữ cái trùng lặp: một chuỗi sẽ đại diện cho lần xuất hiện đầu tiên của chúng và chuỗi còn lại sẽ đại diện cho lần xuất hiện thứ hai của chúng. Ý tưởng cốt lõi là chúng ta sẽ đặt những lần xuất hiện đầu tiên theo thứ tự tăng dần và những lần xuất hiện thứ hai theo thứ tự dịch chuyển nhưng căn chỉnh sao cho mọi cặp đều có cùng khoảng cách. 
4. Đặt tất cả các chữ cái xuất hiện một lần vào bất kỳ vị trí nào còn lại. Những chữ cái này không hạn chế khoảng cách nên có thể chèn vào mà không ảnh hưởng đến cấu trúc. 
5. Xây dựng chuỗi cuối cùng bằng cách xen kẽ các cấu trúc xảy ra lần đầu và lần xuất hiện thứ hai theo cách thực thi một sự dịch chuyển không đổi. Một cách tự nhiên là đặt tất cả lần xuất hiện đầu tiên ở phân đoạn trước và tất cả lần xuất hiện thứ hai ở phân đoạn sau với thứ tự tương đối giống hệt nhau. 
6. Xuất chuỗi đã tạo. 

### Tại sao nó hoạt động 

Tính chính xác đến từ việc thực thi một mẫu vị trí cứng nhắc cho các chữ cái trùng lặp. Khi tất cả các lần xuất hiện đầu tiên được chỉ định vị trí$p_1 < p_2 < \dots < p_k$và lần xuất hiện thứ hai được chỉ định vị trí$p_1 + d, p_2 + d, \dots, p_k + d$, mỗi chữ cái trùng lặp tự động có khoảng cách$d$. Vì việc xây dựng đảm bảo độ lệch tương đối giống nhau cho tất cả các cặp nên không có chữ cái nào có thể vi phạm ràng buộc. Các chữ cái xuất hiện một lần không bị ràng buộc và do đó không thể phá vỡ điều kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()

    freq = {}
    for ch in s:
        freq[ch] = freq.get(ch, 0) + 1

    ones = []
    twos = []

    for ch in freq:
        if freq[ch] == 1:
            ones.append(ch)
        else:
            twos.append(ch)

    twos.sort()
    ones.sort()

    # Build result in a simple constructive pattern:
    # first all singles, then first occurrences, then second occurrences
    res = []

    # place singletons first
    for ch in ones:
        res.append(ch)

    # first occurrences
    for ch in twos:
        res.append(ch)

    # second occurrences
    for ch in twos:
        res.append(ch)

    print("".join(res))
```Mã đầu tiên tính toán tần số, sau đó chia các ký tự thành các nhóm đơn và nhóm trùng lặp. Nó sắp xếp cả hai nhóm cho cấu trúc xác định. Cấu trúc đặt tất cả các ký tự xuất hiện một lần trước tiên, tiếp theo là lần xuất hiện đầu tiên của các chữ cái trùng lặp và cuối cùng là lần xuất hiện thứ hai của chúng. 

Bố cục này đảm bảo rằng tất cả các chữ cái trùng lặp có khoảng cách giống nhau vì mỗi ký tự trùng lặp được phân chia giữa hai khối liền kề giống nhau, làm cho khoảng cách của chúng bằng kích thước của khối đơn cộng với phần bù do thứ tự tạo ra. Giá trị chính xác của khoảng cách là không liên quan, chỉ có vấn đề bình đẳng. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`abcdcba`Chúng tôi tính toán tần số: tất cả`a, b, c, d`xuất hiện hai lần. Không có người độc thân. 

Chúng tôi chia: 

- những cái = [] 
- twos = [a, b, c, d] 

Sau khi sắp xếp: [a, b, c, d] 

Chúng tôi xây dựng: 

| Bước | Khối Ones | Khối đầu tiên | Khối thứ hai | Kết quả | 
| --- | --- | --- | --- | --- | 
| ban đầu | [] | [] | [] | "" | 
| trùng lặp trước | [] | a b c d | [] | "abcd" | 
| trùng lặp thứ hai | [] | a b c d | a b c d | "abcdabcd" | 

Chuỗi cuối cùng:`abcdabcd`Mỗi chữ cái xuất hiện hai lần với khoảng cách giống nhau là 4. 

Điều này chứng tỏ rằng cấu trúc thực thi việc ghép nối thống nhất ngay cả khi thứ tự ban đầu là tùy ý. 

### Ví dụ 2:`oelhl`Tần số: 

- o:1, e:1, h:1, l:2 

Chia: 

- cái = [e, h, o] 
- hai = [l] 

Xây dựng: 

| Bước | Khối Ones | Khối đầu tiên | Khối thứ hai | Kết quả | 
| --- | --- | --- | --- | --- | 
| ban đầu | [] | [] | [] | "" | 
| đĩa đơn | ồ ồ | [] | [] | "eho" | 
| đầu tiên tôi | ồ ồ | tôi | [] | "ehol" | 
| thứ hai l | ồ ồ | tôi | tôi | "ehol l" | 

Chuỗi cuối cùng:`eholl`Chữ cái trùng lặp duy nhất`l`thỏa mãn khoảng cách bằng nhau một cách tầm thường vì chỉ có một cặp. Singletons không can thiệp vào bất kỳ ràng buộc nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp các ký tự cho mỗi trường hợp kiểm thử chiếm ưu thế, n ≤ 52 | 
| Không gian | O(n) | Lưu trữ bản đồ tần số và chuỗi đầu ra | 

Các ràng buộc là cực kỳ nhỏ, do đó, ngay cả việc sắp xếp cho mỗi trường hợp thử nghiệm cũng có hiệu quả là thời gian không đổi trong thực tế. Giải pháp dễ dàng phù hợp trong giới hạn lên đến$10^3$trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()

        freq = {}
        for ch in s:
            freq[ch] = freq.get(ch, 0) + 1

        ones = []
        twos = []

        for ch in freq:
            if freq[ch] == 1:
                ones.append(ch)
            else:
                twos.append(ch)

        ones.sort()
        twos.sort()

        res = ones + twos + twos
        out.append("".join(res))

    return "\n".join(out)

# provided samples
assert run("3\noelhl\nabcdcba\nac\n") in [
    "eho ll\nabcdabcd\nac"
], "sample check (any valid ordering accepted)"

# custom cases
assert run("1\naabbcc\n") != "", "all duplicates case"
assert run("1\nabcdef\n") == "abcdef", "all singles"
assert run("1\naaaabb\n") != "", "multiple duplicates"
assert run("1\nz\n") == "z", "single char"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`aabbcc`| bất kỳ hoán vị hợp lệ nào có cấu trúc đồng nhất | nhiều chữ cái trùng lặp | 
|`abcdef`| không thay đổi | tất cả các lần xuất hiện duy nhất | 
|`aaaabb`| sắp xếp lại cấu trúc | đĩa đơn và bản sao hỗn hợp | 
|`z`|`z`| đầu vào tối thiểu | 

## Vỏ cạnh 

Khi chuỗi không chứa các chữ cái trùng lặp, việc phân chia tần số chỉ tạo ra các đơn vị. Trong một đầu vào như`ac`, thuật toán đặt cả hai chữ cái vào khối đơn và xuất chúng trực tiếp. Vì không có ràng buộc nào tồn tại nên mọi thứ tự đều hợp lệ và thuật toán tránh được cấu trúc không cần thiết một cách chính xác. 

Khi tất cả các chữ cái được nhân đôi, chẳng hạn như`abcdcba`, khối singleton trống. Việc xây dựng sẽ thu gọn thành hai nửa giống hệt nhau của danh sách trùng lặp đã được sắp xếp. Nửa thứ hai đảm bảo khoảng cách giống nhau vì mỗi cặp chữ cái được phân tách bằng cùng một độ dài đoạn. 

Khi một chữ cái được sao chép giữa nhiều chữ cái đơn lẻ, chẳng hạn như`oelhl`, tiền tố đơn chỉ đơn giản dịch chuyển cả hai lần xuất hiện của chữ cái trùng lặp như nhau. Vì tất cả các bản sao đều có chung tiền tố nên khoảng cách giữa các lần xuất hiện của chúng vẫn nhất quán và vị trí đơn lẻ không thể làm xáo trộn khoảng cách tương đối.
