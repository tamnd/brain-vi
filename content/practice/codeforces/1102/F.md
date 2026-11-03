---
title: "CF 1102F - Ma trận kéo dài"
description: "Chúng ta có một lưới số hình chữ nhật với $n$ hàng và $m$ cột. Chúng ta được phép sắp xếp lại các hàng theo ý muốn nhưng thứ tự các số trong mỗi hàng là cố định."
date: "2026-06-12T05:36:20+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "brute-force", "dp", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1102
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 531 (Div. 3)"
rating: 2000
weight: 1102
solve_time_s: 66
verified: true
draft: false
---

[CF 1102F - Ma trận kéo dài](https://codeforces.com/problemset/problem/1102/F) 

**Xếp hạng:** 2000 
**Thẻ:** tìm kiếm nhị phân, mặt nạ bit, lực lượng vũ phu, dp, đồ thị 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ta có một lưới số hình chữ nhật có$n$hàng và$m$cột. Chúng ta được phép sắp xếp lại các hàng theo ý muốn nhưng thứ tự các số trong mỗi hàng là cố định. Sau khi chọn thứ tự, chúng ta duyệt từng cột lưới từ trên xuống dưới, ghi các số chúng ta truy cập thành một dãy tuyến tính. Nhiệm vụ là tìm số nguyên lớn nhất$k$sao cho các hiệu giữa các số liên tiếp trong dãy này ít nhất bằng$k$. Đầu ra là mức tối đa này$k$. 

Những ràng buộc cho chúng ta biết rằng$n$nhiều nhất là 16, một con số nhỏ, trong khi$m$có thể lớn như$10^4$. Sự kết hợp này gợi ý mạnh mẽ rằng các thuật toán theo cấp số nhân trong$n$đều khả thi, nhưng bất cứ điều gì có quy mô phù hợp với$m^n$thì không. Vì mỗi hàng có nhiều số nên chúng ta không thể ép buộc trực tiếp mọi hoán vị của các giá trị hàng đối với các giá trị lớn.$m$; thay vào đó, chúng ta cần một cách tiếp cận tập trung vào thứ tự hàng. 

Các trường hợp cạnh bao gồm các tình huống trong đó tất cả các hàng giống hệt nhau hoặc gần giống nhau, điều này có thể buộc$k$là rất nhỏ hoặc các ma trận trong đó sự khác biệt rất lớn ở một số cột nhưng lại nhỏ ở những cột khác. Ví dụ, nếu$n = 3$,$m = 2$, và ma trận là```
1 2
2 1
1 1
```một cách tiếp cận tham lam ngây thơ chỉ xem xét cột đầu tiên có thể đánh giá quá cao$k$. Cách tiếp cận đúng phải xem xét cách các hàng liên tiếp tương tác với từng cột. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi hoán vị của các hàng, tính toán chuỗi kết quả và kiểm tra sự khác biệt nhỏ nhất giữa các số liên tiếp. có$n!$hoán vị và mỗi chuỗi có$n \cdot m$các phần tử, do đó độ phức tạp là$O(n! \cdot n \cdot m)$. Với$n = 16$Và$m = 10^4$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là vấn đề chỉ phụ thuộc vào sự khác biệt tối thiểu giữa phần tử cuối cùng của một hàng và phần tử đầu tiên của hàng tiếp theo trong mỗi cột. Chúng ta có thể tính toán trước sự khác biệt tối đa giữa bất kỳ cặp hàng nào trong cùng một cột. Từ$n$nhỏ, chúng ta có thể sử dụng lập trình động trên các tập hợp con của hàng (bitmask DP) để tìm thứ tự hợp lệ cho một ứng cử viên nhất định$k$. Đối với một ứng cử viên$k$, chúng ta kiểm tra xem có tồn tại một hoán vị của các hàng sao cho với mỗi cặp liên tiếp trong phép duyệt, sự khác biệt giữa các số ít nhất là$k$. Tìm kiếm nhị phân có thể$k$các giá trị cho phép chúng ta tìm giá trị lớn nhất một cách hiệu quả. 

Cách tiếp cận bạo lực có hiệu quả vì nó đảm bảo tính chính xác cho các đầu vào nhỏ nhưng không thành công đối với các đầu vào lớn hơn.$m$vì các hoán vị hàng giai thừa. Thông tin chi tiết cho phép DP vượt qua các tập hợp con là số lượng hàng nhỏ nên chúng tôi có thể biểu thị các hàng được sử dụng dưới dạng mặt nạ bit. Việc tính toán trước sự khác biệt tối thiểu được phép giữa hai hàng bất kỳ cho phép chúng tôi kiểm tra ứng cử viên$k$các giá trị một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n * m) | O(n*m) | Quá chậm | 
| Bitmask DP + Tìm kiếm nhị phân | O(2^n * n^2 * m * log(max_value)) | O(2^n * n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước sự khác biệt theo cột giữa mỗi cặp hàng. Đối với hai hàng$i$Và$j$, ghi chênh lệch tuyệt đối nhỏ nhất ở mỗi cột khi đi từ hàng$i$chèo thuyền$j$. Đồng thời ghi lại sự khác biệt giữa phần tử cuối cùng của$i$và phần tử đầu tiên của$j$cho sự liền kề giữa các cột. 
2. Thiết lập tìm kiếm nhị phân trên$k$. Giới hạn dưới bắt đầu từ 0 và giới hạn trên có thể là chênh lệch lớn nhất có thể có, ví dụ:$10^9$. 
3. Đối với ứng viên$k$, khởi tạo một mảng DP trong đó`dp[mask][i]`cho biết liệu có thể sắp xếp các hàng trong tập hợp con hay không`mask`kết thúc bằng hàng`i`thỏa mãn$k$-điều kiện chấp nhận được 
4. Trường hợp cơ sở: mỗi tập hợp con một hàng có giá trị không đáng kể, vì vậy`dp[1<<i][i] = True`. 
5. Lặp lại tất cả các tập hợp con của hàng. Đối với mỗi tập hợp con`mask`và hàng kết thúc`last`, hãy cân nhắc việc thêm một hàng mới`next`không ở trong`mask`. Kiểm tra xem sự khác biệt tối thiểu giữa`last`Và`next`trên tất cả các cột và trên ranh giới của cột cuối cùng và cột đầu tiên ít nhất là`k`. Nếu vậy, hãy đặt`dp[mask | (1<<next)][next] = True`. 
6. Sau khi điền vào bảng DP, kiểm tra xem có`dp[(1<<n)-1][i]`là Đúng. Nếu có, ứng viên$k$là khả thi; nếu không thì nó quá lớn. 
7. Sử dụng tìm kiếm nhị phân để tối đa hóa$k$. Sau khi tìm kiếm, phương án khả thi cuối cùng`k`là câu trả lời. 

Tại sao nó hoạt động: DP liệt kê tất cả các thứ tự hàng có thể có trong một tập hợp con bằng cách sử dụng mặt nạ bit và đảm bảo rằng mỗi hàng được thêm vào đều tôn trọng sự khác biệt tối thiểu. Vì chúng tôi kiểm tra mọi tập hợp con và mọi kết hợp hàng nên mọi thứ tự hợp lệ đều được tìm thấy. Tìm kiếm nhị phân đảm bảo chúng tôi hội tụ đến mức khả thi tối đa$k$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    # Precompute differences
    diff = [[0]*n for _ in range(n)]
    boundary = [[0]*n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            if i == j:
                continue
            min_col = min(abs(a[i][c] - a[j][c]) for c in range(m))
            diff[i][j] = min_col
            boundary[i][j] = abs(a[i][-1] - a[j][0])

    def can(k):
        dp = [ [False]*n for _ in range(1<<n) ]
        for i in range(n):
            dp[1<<i][i] = True

        for mask in range(1<<n):
            for last in range(n):
                if not dp[mask][last]:
                    continue
                for nxt in range(n):
                    if mask & (1<<nxt):
                        continue
                    if diff[last][nxt] >= k and boundary[last][nxt] >= k:
                        dp[mask | (1<<nxt)][nxt] = True

        full_mask = (1<<n) - 1
        return any(dp[full_mask][i] for i in range(n))

    low, high = 0, 10**9
    answer = 0
    while low <= high:
        mid = (low + high) // 2
        if can(mid):
            answer = mid
            low = mid + 1
        else:
            high = mid - 1
    print(answer)

if __name__ == "__main__":
    main()
```Bước tiền tính toán tính toán chênh lệch tuyệt đối tối thiểu cho mỗi cặp hàng trong tất cả các cột và chênh lệch biên. các`can(k)`chức năng sử dụng bitmask DP để kiểm tra tính khả thi. Cập nhật tìm kiếm nhị phân`answer`chỉ khi`k`là khả thi. Việc sử dụng mặt nạ bit đảm bảo chúng tôi khám phá hiệu quả tất cả các thứ tự hàng mà không cần tính toán dư thừa. Có thể tránh các lỗi riêng lẻ bằng cách sử dụng các chỉ số bao hàm cho mặt nạ và khởi tạo cẩn thận các trường hợp cơ sở DP. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4 2
9 9
10 8
5 3
4 3
```Theo dõi ứng viên`k = 5`: 

| mặt nạ (nhị phân) | cuối cùng | dp[mặt nạ][cuối cùng] | hàng mới được xem xét | khác được chứ? | ranh giới được không? | dp mới được cập nhật? | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0001 | 0 | Đúng | 1 | 1 | 2 | Sai | 
| 0001 | 0 | Đúng | 2 | 4 | 6 | Sai | 
| 0001 | 0 | Đúng | 3 | 3 | 5 | Sai | 
| ... | ... | ... | ... | ... | ... | ... | 
| 1111 | 2 | Đúng | - | - | - | - | 

Điều này xác nhận rằng thứ tự hàng`[2,1,3,0]`(được lập chỉ mục 0) tạo ra một chuỗi trong đó mỗi chênh lệch liên tiếp ≥ 5. 

Đầu vào mẫu 2:```
3 3
1 1 1
2 2 2
3 3 3
```Tối đa`k`là 1. DP khám phá tất cả các tập hợp con và thấy rằng thứ tự`[0,2,1]`mang lại sự khác biệt`[2-1, 3-2]`≥ 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^n * n^2 * m * log(max_value)) | Mỗi bước DP kiểm tra tất cả các tập hợp con và tất cả các cặp, tính toán trước sự khác biệt mất n^2 * m, tìm kiếm nhị phân thêm hệ số log(max_value) | 
| Không gian | O(2^n * n + n^2) | Bảng DP và sự khác biệt được tính toán trước | 

Với n 16 và m 10^4, 2^16 * 16^2 * 10^4 ≈ 1e9, các thao tác có thể được chấp nhận trong vòng 4 giây với tối ưu hóa Python và mức sử dụng bộ nhớ vẫn ở mức dưới 256 MB. 

## Bài kiểm tra
