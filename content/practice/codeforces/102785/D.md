---
title: "CF 102785D - Chúng tôi đang cố gắng chia sẻ một quả cam ..."
description: "Quả cam chỉ được mô tả qua số lát nó có. Một công ty có x người có thể chia quả cam một cách chính xác khi số lát chia hết cho x, do đó quy mô công ty hợp lệ chính xác là ước số dương của số lát."
date: "2026-07-27T19:37:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102785
codeforces_index: "D"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 18)"
rating: 0
weight: 102785
solve_time_s: 75
verified: true
draft: false
---

[CF 102785D - Chúng tôi đang cố gắng chia sẻ một quả cam ...](https://codeforces.com/problemset/problem/102785/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Quả cam chỉ được mô tả qua số lát nó có. Một công ty với`x`mọi người có thể chia quả cam một cách chính xác khi số lát chia hết cho`x`, do đó quy mô công ty hợp lệ chính xác là các ước số dương của số lát. 

Nhiệm vụ là tìm số nguyên dương nhỏ nhất`m`số ước của nó chính xác là`k`. Nếu không có số đó tồn tại, câu trả lời sẽ là`-1`. Trong bài toán này, mọi số nguyên dương đều có ít nhất một ước số, do đó luôn tồn tại một câu trả lời hợp lệ cho mọi số nguyên dương.`k`trong phạm vi nhất định. 

Giá trị đầu vào`k`nhiều nhất là 1000. Con số này đủ nhỏ để cho phép các thuật toán phụ thuộc vào các yếu tố của`k`, nhưng nó loại trừ việc kiểm tra mọi kích thước màu cam có thể có. Số nhỏ nhất có nhiều ước số có thể trở nên rất lớn, vì vậy việc mô phỏng thử các giá trị của`m`và việc đếm các ước số của chúng sẽ nhanh chóng trở nên không thực tế. 

Một số trường hợp cần được chăm sóc. Khi`k = 1`, quả cam phải có đúng một ước số, điều này chỉ xảy ra với`m = 1`. Một giải pháp bắt đầu tìm kiếm từ`2`sẽ thất bại không chính xác trong trường hợp này. Đối với đầu vào`1`, đầu ra đúng là`1`. 

Bẫy thứ hai là cho rằng câu trả lời phải là số nguyên tố hoặc lũy thừa của một số nguyên tố. Ví dụ, khi`k = 4`, câu trả lời là`6`, vì các ước của nó là`1, 2, 3, 6`. Việc tìm kiếm bất cẩn chỉ với các lũy thừa nguyên tố sẽ bỏ lỡ điều này và tạo ra một câu trả lời lớn hơn, chẳng hạn như`8`. 

Một vấn đề khác là thứ tự của số mũ nguyên tố. Vì`k = 12`, hệ số của số chia có thể được biểu diễn bằng`(3 * 2 * 2)`, cho số mũ`(2, 1, 1)`và số`2² * 3 * 5 = 60`. Sử dụng số mũ`(1, 2, 1)`cho`2 * 3² * 5 = 90`, cái nào lớn hơn. Một cấu trúc không đặt số mũ lớn hơn trên các số nguyên tố nhỏ hơn có thể tạo ra số chia chính xác nhưng không phải là số tối thiểu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử từng số nguyên dương. Đối với mỗi ứng viên`m`, chúng ta có thể đếm các ước của nó bằng cách kiểm tra tất cả các giá trị từ`1`ĐẾN`sqrt(m)`. Điều này đúng vì cuối cùng nó đạt đến số nhỏ nhất với chính xác`k`các ước số, nhưng nó không có giới hạn trên hữu ích về khoảng cách nó có thể tìm kiếm. Số lượng ứng cử viên và số chia đều tăng quá nhanh, khiến phương pháp này không thể sử dụng được. 

Quan sát chính xuất phát từ công thức chia. Nếu như`m = p1^a1 * p2^a2 * ... * pt^at`thì số ước số là`(a1 + 1) * (a2 + 1) * ... * (at + 1)`. 

Thay vì tìm kiếm các số có thể, chúng ta có thể tìm kiếm các phân thức có thể có của`k`. Mỗi thừa số trong phép nhân này trở thành một số mũ cộng một. 

Để làm cho số càng nhỏ càng tốt, số mũ lớn nhất phải được gán cho số nguyên tố nhỏ nhất. Nếu chúng ta có số mũ`5`Và`2`, gán chúng cho số nguyên tố`2`Và`3`cho`2^5 * 3^2`, nhỏ hơn`2^2 * 3^5`. Điều này có nghĩa là chúng ta chỉ cần xét các dãy số mũ không tăng. 

Tìm kiếm tối ưu phân chia đệ quy`k`vào các yếu tố. Yếu tố được chọn`d`có nghĩa là số nguyên tố hiện tại nhận được số mũ`d - 1`. Sau khi chọn nó, số ước còn lại sẽ trở thành`k / d`. Bởi vì`k`chỉ là 1000, số lượng các hệ số như vậy là rất ít. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(câu trả lời × sqrt(câu trả lời)) | O(1) | Quá chậm | 
| Tối ưu | O(số phân vùng nhân của k) | O(số thừa số nguyên tố của k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý trường hợp đặc biệt`k = 1`. Số duy nhất có một ước là`1`, vậy là có ngay câu trả lời`1`. 
2. Bắt đầu tìm kiếm đệ quy với số ước còn lại bằng`k`, số nguyên tố có sẵn đầu tiên bằng`2`và số được xây dựng hiện tại bằng`1`. 
3. Chọn hệ số`d`của số chia còn lại. Yếu tố này thể hiện giá trị`exponent + 1`đối với số nguyên tố hiện tại, hãy nhân số hiện tại với`prime^(d - 1)`. 
4. Lặp lại phần còn lại`remaining / d`, chuyển sang số nguyên tố tiếp theo. Hạn chế hệ số được chọn tiếp theo không lớn hơn`d`, bởi vì thừa số phải không tăng để giữ số mũ không tăng. 
5. Khi số ước còn lại trở thành`1`, tất cả số mũ cần thiết đã được chỉ định. So sánh số được xây dựng với câu trả lời tốt nhất được tìm thấy cho đến nay. 

Việc tìm kiếm chỉ khám phá các phân tích hợp lệ của`k`. Mỗi đường dẫn tạo ra một số có số chia chính xác`k`và hạn chế thứ tự đảm bảo rằng không có phép phân tích nhân tử tương đương nào bị bỏ qua theo cách có thể tạo ra kết quả nhỏ hơn. 

Tại sao nó hoạt động: Mỗi số nguyên dương có một hệ số nguyên tố duy nhất, vì vậy mọi câu trả lời có thể tương ứng với chính xác một chuỗi số mũ. Công thức chia chuyển đổi yêu cầu thành hệ số của`k`. Tìm kiếm đệ quy liệt kê tất cả các phân vùng nhân có thể có của`k`và gán số mũ lớn hơn cho các số nguyên tố nhỏ hơn sẽ cho số tối thiểu cho mỗi phân vùng. Vì mọi cách xây dựng hợp lệ đều được kiểm tra nên giá trị nhỏ nhất được ghi lại là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    k = int(input())

    if k == 1:
        print(1)
        return

    primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31]
    best = [10 ** 100]

    def dfs(rem, idx, max_factor, cur):
        if rem == 1:
            if cur < best[0]:
                best[0] = cur
            return

        if idx >= len(primes):
            return

        p = primes[idx]

        for factor in range(min(max_factor, rem), 1, -1):
            if rem % factor == 0:
                nxt = cur * (p ** (factor - 1))
                if nxt < best[0]:
                    dfs(rem // factor, idx + 1, factor, nxt)

    dfs(k, 0, k, 1)

    print(best[0] if best[0] != 10 ** 100 else -1)

if __name__ == "__main__":
    solve()
```Danh sách nguyên tố chứa đủ các số nguyên tố nhỏ vì`k`tối đa là 1000. Số lượng thừa số tối đa có thể có trong một phân vùng nhân bị giới hạn bằng cách nhân liên tục với`2`, vì vậy chỉ có thể cần một số lượng nhỏ số nguyên tố. 

Hàm đệ quy lưu trữ phần còn lại của`k`, chỉ số của số nguyên tố tiếp theo, thừa số cho phép lớn nhất và số được xây dựng cho đến nay. các`max_factor`đối số ngăn chặn việc tạo cùng một tập hợp số mũ theo các thứ tự khác nhau. 

Vòng lặp thử các thừa số từ lớn đến nhỏ. Bản thân thứ tự này không cần thiết để đảm bảo tính đúng đắn, nhưng nó có xu hướng sớm tìm ra những ứng cử viên mạnh, điều này làm cho điều kiện cắt tỉa hiệu quả hơn. Việc cắt tỉa là an toàn vì bất kỳ sự tiếp tục nào cũng chỉ nhân lên`cur`bằng số lớn hơn`1`, do đó, một phần giá trị đã lớn hơn câu trả lời tốt nhất không thể cải thiện được. 

Số nguyên Python không bị tràn, điều này tránh được rủi ro triển khai chính trong vấn đề này. Độ sâu tìm kiếm cũng rất nhỏ vì số lượng yếu tố của`k`không thể phát triển nhiều. 

## Ví dụ đã hoạt động 

Đối với đầu vào`1`, việc tìm kiếm không cần đệ quy. 

| Đầu vào k | Hành động | Câu trả lời hiện tại | 
| --- | --- | --- | 
| 1 | Trường hợp đặc biệt, chỉ có một ước số | 1 | 

Điều này xác nhận đầu vào tối thiểu có thể và tránh sai lầm phổ biến là cho rằng mọi câu trả lời đều phải lớn hơn một. 

Đối với đầu vào`6`, các phân vùng nhân được khám phá. 

| Còn lại k | Yếu tố được chọn | Prime đã qua sử dụng | Số hiện tại | 
| --- | --- | --- | --- | 
| 6 | 3 | 2 | 4 | 
| 2 | 2 | 3 | 12 | 
| 1 | dừng lại | | 12 | 
| 6 | 2 | 2 | 2 | 
| 3 | 3 | 3 | 18 | 
| 1 | dừng lại | | 18 | 

Giá trị tốt nhất được tìm thấy là`12`. Các ước số của nó là`1, 2, 3, 4, 6, 12`, đưa ra chính xác sáu quy mô công ty có thể có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(P) | P là số phân vùng nhân của`k`, nhỏ đối với`k ≤ 1000`| 
| Không gian | O(logk) | Độ sâu đệ quy bằng số thừa số nguyên tố được sử dụng | 

Thuật toán không bao giờ lặp qua các kích thước màu cam có thể có. Nó chỉ khám phá các hệ số của số chia, vì vậy nó dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp):
    k = int(inp)

    if k == 1:
        return "1"

    primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31]
    best = [10 ** 100]

    def dfs(rem, idx, max_factor, cur):
        if rem == 1:
            best[0] = min(best[0], cur)
            return
        for factor in range(min(max_factor, rem), 1, -1):
            if rem % factor == 0:
                dfs(rem // factor, idx + 1, factor,
                    cur * primes[idx] ** (factor - 1))

    dfs(k, 0, k, 1)
    return str(best[0])

assert solution("1") == "1", "minimum input"
assert solution("4") == "6", "small composite divisor count"
assert solution("6") == "12", "sample-style case"
assert solution("12") == "60", "different exponent ordering"
assert solution("1000") == "810810000", "maximum input size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | Xử lý số có một ước duy nhất | 
| 4 | 6 | Tránh chỉ chọn sai lũy thừa nguyên tố | 
| 6 | 12 | Kiểm tra phân vùng hệ số bình thường | 
| 12 | 60 | Kiểm tra thứ tự số mũ để xây dựng tối thiểu | 
| 1000 | 810810000 | Xác nhận tìm kiếm xử lý hạn chế tối đa | 

## Vỏ cạnh 

cho`k = 1`, thuật toán trả về ngay`1`mà không cần nhập tìm kiếm đệ quy. số`1`chỉ có một ước số duy nhất nên kết quả đầu ra là chính xác. 

Vì`k = 4`, tìm kiếm đệ quy có thể chia số chia thành`2 * 2`. Điều này tạo ra số mũ`1`Và`1`, cho số`2 * 3 = 6`. Thuật toán không bị mắc kẹt bởi lũy thừa nguyên tố lớn hơn`2^3 = 8`, bởi vì nó đánh giá tất cả các phân vùng nhân. 

Vì`k = 12`, một sự phân chia có thể là`3 * 2 * 2`, tạo số mũ`2, 1, 1`. Thuật toán gán chúng cho`2, 3, 5`, sản xuất`60`. Nếu số mũ được gán theo thứ tự khác, số chia vẫn đúng nhưng giá trị sẽ lớn hơn, đó là lý do tại sao việc hạn chế hệ số giảm là cần thiết.
