---
title: "CF 102215K - Sắp xếp bộ bài"
description: "Chúng ta có một chuỗi có độ dài (n), trong đó mỗi ký tự là một trong các R, G hoặc B. Chuỗi mô tả bộ bài từ trên xuống dưới. Chúng ta có thể chia từng thẻ một thành hai chồng."
date: "2026-08-17T23:51:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "K"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 147
verified: true
draft: false
---

[CF 102215K - Sắp xếp bộ bài](https://codeforces.com/problemset/problem/102215/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi có độ dài (n), trong đó mỗi ký tự là một trong`R`,`G`, hoặc`B`. Chuỗi mô tả bộ bài từ trên xuống dưới. Chúng ta có thể chia từng thẻ một thành hai chồng. Vì mỗi lá bài mới được đặt lên trên chồng bài đã chọn của nó, nên thứ tự bên trong mỗi chồng bài sẽ bị đảo ngược so với thứ tự các lá bài của nó xuất hiện trong bộ bài ban đầu. Cuối cùng, toàn bộ một cọc được đặt chồng lên nhau, vì vậy bộ bài cuối cùng là sự kết hợp của hai chuỗi con đảo ngược của chuỗi ban đầu. 

Mục đích là để quyết định xem bộ bài cuối cùng có thể có tất cả các lá bài cùng màu hay không. Ba khối màu có thể xuất hiện theo thứ tự bất kỳ. Bài toán chính thức cho phép (1 \le n \le 1000). 

Giới hạn chỉ 1000 ký tự là đủ lớn để không thể liệt kê đầy đủ tất cả các cách phân phối thẻ. Có (2^n) phép gán thẻ cho hai cọc, do đó, ngay cả việc kiểm tra một lần gán trong thời gian (O(n)) cũng sẽ cho ra các phép toán (O(n2^n)). Đối với (n=1000), điều này vượt xa giới hạn 2 giây có thể xử lý. Thay vào đó, giải pháp nên khai thác thực tế là chỉ có ba màu, chỉ đưa ra sáu thứ tự có thể có cho các khối màu cuối cùng. 

Có một số trường hợp ranh giới có thể đánh lừa việc thực hiện bất cẩn. Bộ bài một lá bài chẳng hạn như`R`phải quay lại`YES`, bởi vì nó đã được sắp xếp và không cần phân chia có ý nghĩa. Việc triển khai giả định cả ba màu đều xuất hiện có thể không thành công ở đây. Một bộ bài như`RGB`cũng phải quay lại`YES`, mặc dù mỗi màu xuất hiện đúng một lần. Một giải pháp đòi hỏi sự xuất hiện lặp đi lặp lại của mọi màu sắc sẽ loại bỏ nó một cách không chính xác. 

Một màu vắng mặt cũng cần được xử lý đặc biệt. Ví dụ,`RRR`đã được sắp xếp rồi nên câu trả lời là`YES`. Khi màu được chọn làm màu đầu tiên hoặc màu cuối cùng của thứ tự ứng cử viên không xuất hiện, thì không có lần xuất hiện đầu tiên hoặc cuối cùng thực tế nào được sử dụng làm ranh giới. Ví dụ: việc coi một lần xuất hiện đầu tiên không tồn tại là vị trí (n) có thể vô tình đếm các thẻ không thuộc về bên đó. Việc triển khai bên dưới xử lý các màu không tồn tại một cách rõ ràng. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp có thể gán từng lá bài một cách độc lập cho một trong hai cọc. Đối với mỗi phép gán trong số (2^n), chúng ta có thể xây dựng hai cọc, đảo ngược thứ tự bên trong của chúng, ghép chúng theo cả hai thứ tự có thể và kiểm tra xem bộ bài thu được có bao gồm tối đa ba khối màu hay không. Điều này đúng vì mọi chuỗi thao tác hợp pháp đều tương ứng với việc phân chia thành hai cọc như vậy. Tuy nhiên, có (2^n) phân vùng và mỗi lần kiểm tra sẽ thực hiện (O(n)), cho (O(n2^n)) công việc. Tại (n=1000), riêng số lượng bài tập đã xấp xỉ (10^{301}), vì vậy phương pháp này không khả thi chút nào. 

Quan sát hữu ích là bộ bài cuối cùng chỉ chứa ba khối màu. Giả sử chúng ta chọn ba màu riêng biệt (A,B,C) theo thứ tự nào đó. Chúng ta sẽ dựng hai cọc sao cho sau khi đảo ngược và ghép chúng lại, kết quả là (C^*B^_A^_), đây đã là một bộ bài được sắp xếp hợp lệ. Chỉ có sáu lựa chọn cho (A,B,C). 

Với lựa chọn cố định (A,B,C), đặt mọi thẻ (A) vào cọc đầu tiên và mọi thẻ (C) vào cọc thứ hai. Các thẻ (B) còn lại chỉ có hai vị trí hữu ích. Thẻ A (B) xuất hiện trước thẻ (C) đầu tiên có thể đi vào cọc thứ hai. Lá bài A(B) xuất hiện sau lá bài (A) cuối cùng có thể được xếp vào chồng bài đầu tiên. 

Tại sao chính xác những vị trí này? Cọc đầu tiên khi đọc theo thứ tự bộ bài gốc sẽ có dạng 

[ 
A A \ldots A B B \ldots B. 
] 

Sau khi bị đảo ngược, nó trở thành 

[ 
B B \ldots B A A \ldots A. 
] 

Đống thứ hai có thứ tự ban đầu 

[ 
B B \ldots B C C \ldots C, 
] 

vì vậy sau khi đảo ngược nó trở thành 

[ 
C C \ldots C B B \ldots B. 
] 

Đặt chồng thứ hai lên trên chồng thứ nhất sẽ có được 

[ 
C C \ldots C B B \ldots B A A \ldots A, 
] 

được sắp xếp. 

Câu hỏi duy nhất là liệu mọi lá bài (B) có thể được chia vào một trong hai cọc này mà không cần chia cùng một lá bài hai lần hay không. A (B) trước (C) đầu tiên thuộc về cọc thứ hai, trong khi (B) sau (A) cuối cùng thuộc về cọc thứ nhất. Nếu một số (B) nằm giữa cái đầu tiên (C) và cái cuối cùng (A), thì nó không thể được đặt theo cấu trúc này. Nếu hai vùng trùng nhau thì a(B) cũng có thể được tính hai lần. Như vậy việc xây dựng thành công chính xác khi số lượng quân bài được gán cho hai cọc là (n). 

Đặc tính này cũng đủ cho mọi cách sắp xếp hợp lệ có thể. Nếu thứ tự màu cuối cùng là (X,Y,Z), lấy (A=Z), (B=Y) và (C=X). Việc cắt giữa hai cọc cuối cùng có thể xảy ra ở bất kỳ đâu bên trong khối màu ở giữa. Đảo ngược hai quân cờ đặt các quân bài (Y) thuộc quân đầu tiên trước (X) vào bộ bài ban đầu và các quân bài (Y) còn lại sau (Z), đây chính xác là cấu trúc được thử nghiệm bởi cấu trúc ở trên. Việc liệt kê tất cả sáu hoán vị bao gồm mọi thứ tự màu cuối cùng có thể có. Ý tưởng cấu trúc tương tự được sử dụng trong các giải pháp được công bố cho vấn đề này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n2^n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem mỗi lần bao nhiêu lần`R`,`G`, Và`B`xảy ra. Đồng thời ghi lại lần xuất hiện đầu tiên và cuối cùng của mỗi màu. Các giá trị này cho phép chúng tôi kiểm tra mọi thứ tự ứng viên mà không cần tìm kiếm nhiều lần trong chuỗi. 
2. Liệt kê tất cả sáu hoán vị ((A,B,C)) của ba màu. Chúng tôi hiểu (A) là màu được đặt ở cuối bộ bài được sắp xếp cuối cùng, (C) là màu được đặt ở trên cùng và (B) là màu ở giữa. 
3. Xếp từng thẻ (A) vào cọc một và mọi thẻ (C) vào cọc hai. Điều này sửa hai khối màu bên ngoài. 
4. Đếm các thẻ (B) trước thẻ (C) đầu tiên. Những thẻ này có thể được xếp vào cọc số hai một cách an toàn. Sau khi đảo ngược cọc đó, chúng sẽ xuất hiện sau tất cả các lá bài (C). 
5. Đếm các thẻ (B) sau thẻ (A) cuối cùng. Những thẻ này có thể được xếp vào cọc một một cách an toàn. Sau khi đảo ngược cọc đó, chúng sẽ xuất hiện trước tất cả các thẻ (A). 
6. Thêm số thẻ (A), thẻ (C) và hai nhóm thẻ (B) có thể sử dụng được. Nếu tổng số này chính xác là (n), thì mỗi thẻ đã được chỉ định chính xác một lần, do đó thứ tự đã chọn sẽ đưa ra cách sắp xếp hợp lệ. Nếu tổng nhỏ hơn (n) thì một số quân bài (B) nằm ở vùng cấm. Nếu nó lớn hơn (n), một số lá bài (B) đã được tính ở cả hai mặt. Cả hai trường hợp đều có nghĩa là hoán vị này không hoạt động. 
7. Nếu bất kỳ hoán vị nào trong sáu hoán vị thành công, hãy in`YES`. Nếu không thành công, hãy in`NO`. 

Khi một màu không xuất hiện, ranh giới cần được giải thích có chủ ý. Nếu (C) vắng mặt thì không có lá bài (C) đầu tiên, do đó không có lá bài (B) nào có thể được xếp vào loại trước lá bài (C) đầu tiên. Nếu (A) vắng mặt thì không có (A) cuối cùng, nên mọi thẻ (B) đều nằm sau (A) cuối cùng. Mã đại diện trực tiếp cho hai trường hợp này với`first_c = n`Và`last_a = -1`, sau đó xử lý số lượng tương ứng một cách rõ ràng. 

### Tại sao nó hoạt động 

Đối với hoán vị cố định (A,B,C), cách xây dựng gán cho cọc một chuỗi tất cả các thẻ (A), theo sau là các thẻ (B) được chọn sau thẻ (A) cuối cùng. Do đó, sự đảo ngược của nó là một khối thẻ (B) theo sau là một khối thẻ (A). Cọc hai chứa các thẻ (B) đã chọn trước thẻ (C) đầu tiên, tiếp theo là tất cả các thẻ (C). Sự đảo ngược của nó là một khối thẻ (C) theo sau là một khối thẻ (B). Kết hợp cọc hai với cọc một sẽ tạo ra (C^*B^_A^_). 

Tổng số đếm được bằng (n) chính xác khi mỗi quân bài gốc thuộc về một trong hai cọc này đúng một lần. Do đó mọi hoán vị thành công đều tạo ra một sự sắp xếp hợp lệ. Ngược lại, mọi cách sắp xếp hợp lệ đều có ba khối màu và ranh giới giữa hai cột cuối cùng. Việc chọn (A,B,C) theo thứ tự ngược lại của các khối đó làm cho các quân bài được gán cho hai cọc có cấu trúc ranh giới chính xác được thuật toán kiểm tra. Vì tất cả sáu hoán vị đều được kiểm tra nên mọi cách sắp xếp hợp lệ đều được biểu diễn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from itertools import permutations

def solve(s: str) -> str:
    n = len(s)
    colors = "RGB"

    count = {c: 0 for c in colors}
    first = {c: n for c in colors}
    last = {c: -1 for c in colors}

    for i, ch in enumerate(s):
        count[ch] += 1
        first[ch] = min(first[ch], i)
        last[ch] = i

    # Prefix counts let us count how many B cards occur
    # before an arbitrary position in O(1).
    prefix = {c: [0] * (n + 1) for c in colors}

    for i, ch in enumerate(s):
        for c in colors:
            prefix[c][i + 1] = prefix[c][i]
        prefix[ch][i + 1] += 1

    for a, b, c in permutations(colors):
        taken = count[a] + count[c]

        # B cards before the first C.
        if first[c] != n:
            taken += prefix[b][first[c]]

        # B cards after the last A.
        if last[a] != -1:
            taken += count[b] - prefix[b][last[a] + 1]
        else:
            # No A exists, so every B is after the last A.
            taken += count[b]

        if taken == n:
            return "YES"

    return "NO"

def main():
    s = input().strip()
    print(solve(s))

if __name__ == "__main__":
    main()
```Vòng lặp đầu tiên ghi lại tần số và ranh giới của mọi màu.`first[c]`được khởi tạo thành`n`, trong khi`last[a]`được khởi tạo thành`-1`, do đó, các màu không tồn tại có thể được phát hiện mà không có giá trị trọng điểm đặc biệt nằm ngoài phạm vi chỉ mục hợp lệ. 

Mảng đếm tiền tố làm cho hai vùng (B) dễ đo lường.`prefix[b][first[c]]`đếm các vị trí một cách nghiêm ngặt trước`first[c]`, đó chính xác là những gì cần thiết cho khu vực đầu tiên. Đối với khu vực khác,`count[b] - prefix[b][last[a] + 1]`đếm các vị trí một cách nghiêm ngặt sau`last[a]`. các`+1`là cần thiết vì mảng tiền tố sử dụng phạm vi nửa mở. 

Không có vấn đề tràn số nguyên trong Python và tất cả số đếm tối đa là 1000. Sáu hoán vị được tạo bởi`itertools.permutations`, do đó mọi thứ tự có thể có của ba màu đều được kiểm tra chính xác một lần. 

Việc triển khai sử dụng một cấu trúc đếm tiền tố bổ sung (3(n+1)). Giá trị này nhỏ đối với (n \le 1000) và nó giữ phép thử thực tế cho mỗi thời gian hoán vị không đổi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hãy xem xét`RGBRGB`. Lấy hoán vị (A=R,B=G,C=B). Các ranh giới liên quan là ranh giới cuối cùng`R`ở chỉ số 3 và đầu tiên`B`ở chỉ số 2. 

| Hoán vị | Đếm A | Đếm C | B trước C đầu tiên | B sau A cuối cùng | Đã chụp | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| (R, G, B) | 2 | 2 | 1 | 1 | 6 | CÓ | 

Hai cọc có thể được hiểu trực tiếp. Đống một nhận được tất cả`R`thẻ và`G`thẻ sau thẻ cuối cùng`R`, đưa ra thứ tự ban đầu`RRG`. Cọc hai nhận được`G`trước lần đầu tiên`B`và tất cả`B`thẻ, tặng`GBB`. Đảo ngược chúng tạo ra`BGG`Và`RR`, do đó đặt cọc hai lên trên cọc một sẽ cho`BGGRR`. Chính xác hơn, việc sử dụng các vị trí thực tế sẽ tạo ra cấu trúc hợp lệ của mẫu, bộ bài cuối cùng có ba khối màu liền kề nhau. Điều bất biến chính là mỗi lá bài được gán một lần và mỗi cọc có tối đa hai lần chạy màu trước khi đảo ngược. 

### Mẫu 2 

cho`RGBRGBRGB`, hoán vị (R,G,B) cho các giá trị sau. 

| Hoán vị | Đếm A | Đếm C | B trước C đầu tiên | B sau A cuối cùng | Đã chụp | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| (R, G, B) | 3 | 3 | 1 | 0 | 7 | KHÔNG | 

Hai người còn lại`G`thẻ nằm giữa thẻ đầu tiên`B`và cuối cùng`R`, vì vậy thứ tự cụ thể này không thể chỉ định tất cả các thẻ. Năm hoán vị còn lại không thành công vì lý do cấu trúc tương tự. Không có ranh giới nào có thể có giữa hai cọc có thể chứa được sự lặp đi lặp lại`RGB`mẫu, vậy câu trả lời là`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Số lượng tòa nhà và mảng tiền tố lấy (O(n)) và chỉ có sáu hoán vị được chọn trong (O(1)) mỗi hoán vị. | 
| Không gian | (O(n)) | Ba mảng tiền tố có độ dài (n+1) được lưu trữ. | 

Với (n \le 1000), thuật toán chỉ thực hiện một số lần duyệt không đổi nhỏ qua đầu vào và sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```
# helper: run the core solver directly
def run(inp: str) -> str:
    return solve(inp.strip())

# provided samples
assert run("RGBRGB") == "YES", "sample 1"
assert run("RGBRGBRGB") == "NO", "sample 2"
assert run("RBBRRB") == "YES", "sample 3"

# minimum-size input
assert run("R") == "YES", "single card is already sorted"

# all-equal values
assert run("GGGGGG") == "YES", "one color is already one continuous block"

# maximum-size input
assert run("R" * 1000) == "YES", "maximum n with one color"

# boundary case where the first candidate has its C at position 0
assert run("BGR") == "YES", "different colors can already form three blocks"

# repeated pattern that cannot be handled by two reversed piles
assert run("RGBRGBRGB") == "NO", "three repetitions expose the forbidden middle region"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`R`|`YES`| Kích thước tối thiểu và tình trạng thiếu màu | 
|`GGGGGG`|`YES`| Tất cả các thẻ có cùng màu | 
|`R`lặp lại 1000 lần |`YES`| Kích thước đầu vào tối đa | 
|`BGR`|`YES`| Ranh giới xuất hiện đầu tiên và cuối cùng | 
|`RGBRGBRGB`|`NO`| Một mô hình lặp đi lặp lại thực sự không thể | 

## Vỏ cạnh 

Đối với đầu vào một thẻ`R`, thuật toán xem xét ngay các hoán vị chứa`R`. Nếu như`R`được chọn là (A),`count[A] = 1`, trong khi các màu khác có số lượng bằng 0. Không cần thêm thẻ, vì vậy`taken = 1 = n`và câu trả lời là`YES`. Thuật toán không bao giờ giả định rằng có cả ba màu. 

Vì`GGGGGG`, chọn (A=G). Mỗi thẻ đã là một phần của khối bên ngoài được biểu thị bằng (A), vì vậy`count[A] = 6`và tổng số chính xác là (n). Các màu bị thiếu không gây ra bất kỳ sự đếm ngẫu nhiên nào vì màu vắng mặt (C) không đóng góp quân bài nào trước khi nó không tồn tại lần đầu tiên. 

Vì`BGR`, xét (A=R,B=G,C=B). đầu tiên`B`đang ở chỉ số 0 và cuối cùng`R`ở chỉ số 2. Không có`G`thẻ trước thẻ đầu tiên`B`và không`G`thẻ sau lần cuối cùng`R`, vì vậy hoán vị cụ thể này không thành công. Một hoán vị khác thì tương ứng với thứ tự đã được sắp xếp`B-G-R`. Ví dụ này giải thích tại sao tất cả sáu hoán vị phải được kiểm tra thay vì cố định một thứ tự màu. 

Vì`RGBRGBRGB`, mỗi ứng cử viên đặt hàng để lại ít nhất một thẻ có màu ở giữa trong vùng không thuộc cọc theo cấu trúc được yêu cầu. Với (A=R,B=G,C=B), giá trị đầu tiên`B`xảy ra ở chỉ số 2 và cuối cùng`R`ở chỉ số 6. Chỉ có một`G`xảy ra trước lần đầu tiên`B`, và không có điều gì xảy ra sau lần cuối cùng`R`, trong khi có ba`G`tổng số thẻ. Như vậy chỉ có bảy trong số chín lá bài được tính. Năm thứ tự màu còn lại thất bại một cách đối xứng, đưa ra yêu cầu`NO`.
