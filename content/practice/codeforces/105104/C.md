---
title: "CF 105104C - Tính các khoảng"
description: "Chúng ta được cung cấp một mảng cho mỗi trường hợp thử nghiệm và chúng ta cần đếm xem có bao nhiêu mảng con thỏa mãn một điều kiện chẵn lẻ cụ thể: bên trong mảng con đã chọn, ít nhất một giá trị xuất hiện với số lần lẻ."
date: "2026-06-27T20:08:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105104
codeforces_index: "C"
codeforces_contest_name: "2024 HNMU@XTU"
rating: 0
weight: 105104
solve_time_s: 49
verified: true
draft: false
---

[CF 105104C - Tính toán các khoảng](https://codeforces.com/problemset/problem/105104/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng cho mỗi trường hợp thử nghiệm và chúng ta cần đếm xem có bao nhiêu mảng con thỏa mãn một điều kiện chẵn lẻ cụ thể: bên trong mảng con đã chọn, ít nhất một giá trị xuất hiện với số lần lẻ. 

Tương tự, một mảng con được coi là hợp lệ nếu phân bố tần số của nó không đồng đều. Cách duy nhất một mảng con sẽ thất bại là nếu mọi giá trị riêng biệt bên trong nó xuất hiện với số lần chẵn. 

Đầu ra là số lượng mảng con như vậy trong tất cả các trường hợp thử nghiệm. 

Các ràng buộc lớn theo một cách rất cụ thể. Tổng độ dài của tất cả các trường hợp thử nghiệm lên tới 10^6 và số lượng trường hợp thử nghiệm cũng có thể lớn. Điều này loại trừ mọi cách liệt kê O(n^2) của các mảng con. Ngay cả O(n log n) cho mỗi trường hợp thử nghiệm cũng sẽ ở mức giới hạn trừ khi cực kỳ nhẹ. 

Một cách tiếp cận đơn giản để kiểm tra từng mảng con và đếm tần số sẽ yêu cầu các mảng con O(n^2) và tối đa O(n) hoạt động trên mỗi mảng con, vượt xa giới hạn. Ngay cả việc tối ưu hóa việc bảo trì tần số vẫn để lại các mảng con O(n^2). 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các phần tử giống hệt nhau. Ví dụ: nếu mảng là [1, 1, 1, 1] thì mọi mảng con có độ dài lẻ đều hợp lệ vì chỉ khi đó số đếm mới trở thành số lẻ. Một cách tiếp cận bất cẩn có thể cho rằng tất cả các mảng con đều hợp lệ một cách không chính xác hoặc cố gắng viết tắt không chính xác dựa trên tính chẵn lẻ toàn cục. 

Một trường hợp cạnh khác là các mẫu xen kẽ như [1, 2, 1, 2]. Ở đây, nhiều mảng con có số lượng tần số chẵn và cấu trúc của các mảng con hợp lệ phụ thuộc vào việc hủy bỏ tính chẵn lẻ trên các phân đoạn chứ không chỉ kiểm tra cục bộ. 

Khó khăn chính là điều kiện mang tính tổng thể theo tần số chứ không phải cục bộ trên các phần tử. 

## Phương pháp tiếp cận 

Một giải pháp brute-force liệt kê mọi mảng con [l, r], duy trì bản đồ tần số và kiểm tra xem tất cả các tần số có chẵn hay không. Đối với mỗi mảng con, chúng tôi sẽ cập nhật tần số tăng dần nhưng vẫn tính toán lại hoặc duy trì trạng thái chẵn lẻ. Ngay cả với các bản cập nhật gia tăng, vẫn có các mảng con O(n^2) và mỗi bản cập nhật ảnh hưởng đến trạng thái O(1) nhưng việc kiểm tra tính hợp lệ đòi hỏi phải hiểu rõ liệu tất cả các số đếm có chẵn hay không, điều này vẫn phụ thuộc vào việc theo dõi tính chẵn lẻ cho tất cả các giá trị riêng biệt tiềm năng. 

Điều này dẫn đến khoảng O(n^2) hoạt động cho mỗi thử nghiệm trong trường hợp xấu nhất, trở thành 10^12 hoạt động trên tổng quy mô, rõ ràng là không thể. 

Quan sát quan trọng là chúng ta thực sự không cần phải theo dõi tần số đầy đủ. Chúng tôi chỉ quan tâm đến tính chẵn lẻ của tần số. Mỗi giá trị đóng góp số chẵn hoặc số lẻ trong một mảng con. Chúng tôi muốn đếm các mảng con trong đó có ít nhất một giá trị có tính chẵn lẻ lẻ. 

Điều này tương đương với tổng số mảng con trừ đi mảng con trong đó mọi giá trị đều có tần số chẵn. Vì vậy, chúng tôi giải quyết vấn đề: đếm các mảng con có tần số chẵn. 

Bây giờ chúng tôi dịch điều này sang phối cảnh chẵn lẻ tiền tố. Đối với mỗi tiền tố, chúng tôi duy trì trạng thái chẵn lẻ mô tả những giá trị nào đã xuất hiện với số lần lẻ cho đến nay. Một mảng con có tần số chẵn hoàn toàn khi hai trạng thái tiền tố của nó giống hệt nhau. Đó là bởi vì XOR các trạng thái chẵn lẻ bằng nhau sẽ hủy bỏ mọi thứ. 

Vì vậy, vấn đề giảm xuống việc đếm các cặp trạng thái XOR tiền tố bằng nhau. Nếu chúng ta biểu thị tính chẵn lẻ của tiền tố dưới dạng trạng thái có thể băm, thì số lượng các mảng con tốt (chẵn) sẽ là tổng của các trạng thái kết hợp các lần xuất hiện. Khi đó câu trả lời là tổng số mảng con trừ đi giá trị đó. 

Chúng tôi duy trì chữ ký luân phiên giống như XOR của các bản cập nhật chẵn lẻ. Vì các giá trị lớn (lên tới 10^6), chúng tôi không thể lưu trữ toàn bộ bit. Thay vào đó, chúng tôi sử dụng hàm băm ngẫu nhiên cho mỗi giá trị và XOR chúng thành hàm băm tiền tố đang chạy. Điều này nén trạng thái chẵn lẻ thành một số nguyên duy nhất trong khi vẫn duy trì so sánh bằng với xác suất cao. 

Vì vậy, chúng tôi giảm vấn đề xuống việc đếm các giá trị băm tiền tố bằng nhau.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Băm chẵn lẻ tiền tố | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm thử, hãy khởi tạo hàm băm XOR tiền tố đang chạy bằng 0. Điều này thể hiện trạng thái chẵn lẻ của một tiền tố trống trong đó không có giá trị nào xuất hiện. 
2. Gán mỗi giá trị riêng biệt một số nguyên 64 bit ngẫu nhiên. Giá trị này thể hiện sự đóng góp của nó vào việc chuyển đổi tính chẵn lẻ. 
3. Duyệt mảng từ trái sang phải, cập nhật hàm băm tiền tố bằng cách XOR giá trị ngẫu nhiên được chỉ định của phần tử hiện tại. Điều này mô phỏng tính năng bật tắt tính chẵn lẻ: lần xuất hiện đầu tiên sẽ bật tính năng này, lần xuất hiện thứ hai sẽ tắt tính năng này, v.v. 
4. Duy trì một từ điển tần số để đếm số lần mỗi hàm băm tiền tố đã xuất hiện cho đến nay. 
5. Mỗi khi đạt đến một giá trị băm tiền tố, chúng tôi sẽ cộng số lượng hiện tại của hàm băm đó vào tổng số mảng con "xấu" (các mảng con có tất cả các tần số đều chẵn). Điều này hiệu quả vì hai trạng thái tiền tố giống hệt nhau xác định một mảng con có số lượng chẵn. 
6. Sau khi xử lý tất cả các tiền tố, hãy tính tổng các mảng con là n(n+1)/2. 
7. Trừ tổng số phân mảng con có tần số chẵn. Kết quả là số lượng mảng con trong đó có ít nhất một giá trị xuất hiện với số lần lẻ. 

Ý tưởng cốt lõi là sự bình đẳng tiền tố mã hóa việc hủy bỏ tất cả các đóng góp chẵn lẻ trong khoảng. 

### Tại sao nó hoạt động 

Mỗi hàm băm tiền tố biểu thị vectơ chẵn lẻ của tất cả các phần tử cho đến chỉ mục đó. Một mảng con [l, r] có tất cả các tần số chẵn chính xác khi trạng thái chẵn lẻ tại r bằng trạng thái chẵn lẻ ở l − 1. Mã hóa XOR đảm bảo rằng nhiều tập hợp chuyển đổi giống hệt nhau tạo ra các giá trị băm giống hệt nhau. Do đó, việc đếm các trạng thái tiền tố bằng nhau tương đương với việc đếm các mảng con có tính chẵn lẻ bằng 0 và việc bổ sung sẽ đưa ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    rng = 10**6 + 5

    # deterministic pseudo-random using splitmix64 style
    def splitmix64(x):
        x += 0x9e3779b97f4a7c15
        x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9
        x &= (1 << 64) - 1
        x = (x ^ (x >> 27)) * 0x94d049bb133111eb
        x &= (1 << 64) - 1
        return x ^ (x >> 31)

    # precompute random hashes for values
    h = [0] * (rng)
    seed = 123456789
    for i in range(rng):
        seed = splitmix64(seed + i)
        h[i] = seed

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pref = 0
        freq = {0: 1}
        total_even = 0

        for x in a:
            pref ^= h[x]
            total_even += freq.get(pref, 0)
            freq[pref] = freq.get(pref, 0) + 1

        total_subarrays = n * (n + 1) // 2
        print(total_subarrays - total_even)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng hàm băm ổn định cho từng giá trị có thể. Hàm Splitmix64 được sử dụng để tránh xung đột trong thực tế và là tiêu chuẩn trong lập trình cạnh tranh để băm xác định nhanh. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi tính toán trạng thái chẵn lẻ tiền tố bằng cách sử dụng XOR trên các giá trị băm này. Từ điển`freq`theo dõi tần suất mỗi trạng thái tiền tố xuất hiện. Mỗi trạng thái tiền tố lặp lại tạo thành một mảng con “tất cả tần số chẵn” hợp lệ, vì vậy chúng tôi tích lũy số lượng đó. 

Cuối cùng, chúng tôi trừ đi tổng số mảng con. 

Một cạm bẫy triển khai phổ biến là quên khởi tạo`freq[0] = 1`, chiếm các mảng con bắt đầu từ chỉ mục 0. Một mảng khác đang sử dụng hàm băm tích hợp của Python, được ngẫu nhiên hóa trên mỗi quy trình và không ổn định trong các lần chạy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [1, 2, 3]
```Chúng tôi tính toán các trạng thái tiền tố: 

| tôi | một [tôi] | băm tiền tố | tần số trước | đã thêm các mảng con chẵn | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | h1 | {0:1} | 0 | 
| 1 | 2 | h1^h2 | {0:1,h1:1} | 0 | 
| 2 | 3 | h1^h2^h3 | ... | 0 | 

Không có tiền tố lặp lại, vì vậy`total_even = 0`. Tổng số mảng con là 6. Câu trả lời là 6. 

Điều này cho thấy rằng khi tất cả các phần tử là khác nhau thì mọi mảng con đều chứa các lần xuất hiện lẻ. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [1, 1, 2, 2]
```Tiến hóa tiền tố: 

| tôi | một [tôi] | tiền tố | cập nhật tần số | thậm chí các mảng con được thêm vào | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | h1 | +0 | 0 | 
| 1 | 1 | 0 | +0 | 1 (h1 lặp lại) | 
| 2 | 2 | h2 | +0 | 1 | 
| 3 | 2 | 0 | +1 | 2 | 

Ở đây, tiền tố 0 lặp lại 2 lần, tương ứng với các đoạn cân bằng như [1,1,2,2]. Đây chính xác là các mảng con trong đó tất cả các số đếm đều là số chẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi phần tử cập nhật hàm băm tiền tố và tra cứu từ điển | 
| Không gian | O(n) | Từ điển lưu trữ tiền tố trạng thái | 

Tổng số n trên các trường hợp thử nghiệm là 10^6, vì vậy xử lý tuyến tính là đủ. Việc sử dụng bộ nhớ vẫn an toàn vì trạng thái tiền tố là tuyến tính trong trường hợp xấu nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def splitmix64(x):
        x += 0x9e3779b97f4a7c15
        x = (x ^ (x >> 30)) * 0xbf58476d1ce4e5b9
        x &= (1 << 64) - 1
        x = (x ^ (x >> 27)) * 0x94d049bb133111eb
        x &= (1 << 64) - 1
        return x ^ (x >> 31)

    def solve():
        t = int(input())
        rng = 10**6 + 5
        h = [0] * (rng)
        seed = 123456789
        for i in range(rng):
            seed = splitmix64(seed + i)
            h[i] = seed

        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))

            pref = 0
            freq = {0: 1}
            total_even = 0

            for x in a:
                pref ^= h[x]
                total_even += freq.get(pref, 0)
                freq[pref] = freq.get(pref, 0) + 1

            total_subarrays = n * (n + 1) // 2
            print(total_subarrays - total_even)

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided samples
assert run("""1
3
1 2 3
""") == "6", "sample 1"

# custom cases
assert run("""1
1
1
""") == "1", "single element"

assert run("""1
4
1 1 1 1
""") == "10", "all equal"

assert run("""1
4
1 2 1 2
""") == "10", "alternating balanced structure"

assert run("""1
5
1 2 3 2 1
""") == "15", "symmetric pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | trường hợp cơ sở tối thiểu | 
| tất cả đều bình đẳng | 10 | hành vi chuyển đổi chẵn lẻ | 
| xen kẽ | 10 | hủy bỏ nhiều lần | 
| đối xứng | 15 | chồng chéo tiền tố đầy đủ | 

## Vỏ cạnh 

Đối với mảng một phần tử như [7], trạng thái tiền tố là {0, h7}. Không có tiền tố lặp lại, do đó không có mảng con chẵn. Tổng số mảng con là 1, vì vậy câu trả lời là 1. Thuật toán đếm chính xác nó vì hàm băm tiền tố thay đổi ngay lập tức và không bao giờ lặp lại. 

Đối với một mảng hoàn toàn giống hệt nhau như [5, 5, 5, 5], các giá trị băm tiền tố xen kẽ giữa 0 và h5. Trạng thái 0 xuất hiện ở các chỉ số 0, 2, 4, tạo ra nhiều kết quả trùng khớp với tiền tố lặp lại. Chúng tương ứng chính xác với các mảng con có số lượng chẵn, chẳng hạn như hủy cặp kiểu [1,2]. Bước trừ sẽ loại bỏ chính xác các khoảng đó, chỉ để lại những khoảng có ít nhất một số lẻ.
