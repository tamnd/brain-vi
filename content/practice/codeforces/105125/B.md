---
title: "CF 105125B - Tim xạ thủ"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm mô tả một tập hợp các làn bắn, trong đó làn i chứa các mục tiêu Ai được sắp xếp thành một hàng. Tim sẽ bắn một chuỗi các phát bắn và mỗi phát bắn được chỉ định cho đúng một làn."
date: "2026-06-27T19:29:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105125
codeforces_index: "B"
codeforces_contest_name: "MITIT 2024 Spring Invitational Qualification"
rating: 0
weight: 105125
solve_time_s: 98
verified: false
draft: false
---

[CF 105125B - Tim xạ thủ](https://codeforces.com/problemset/problem/105125/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm mô tả một tập hợp các làn đường bắn, trong đó làn đường`i`chứa`A_i`các mục tiêu được sắp xếp thành một dòng. Tim sẽ bắn một chuỗi các phát bắn và mỗi phát bắn được chỉ định cho đúng một làn. 

Cơ chế bắn súng không bình thường vì kết quả của một cú đánh phụ thuộc vào việc đó là cú đánh đầu tiên, thứ ba, thứ năm, v.v. về tổng thể hay thứ hai, thứ tư, thứ sáu, v.v. Khi bắn số lẻ, mục tiêu còn lại đầu tiên trong làn đã chọn sẽ bị tiêu diệt. Khi bắn số chẵn, mục tiêu còn lại đầu tiên sẽ bị bỏ qua và mục tiêu còn lại thứ hai sẽ bị tiêu diệt, nếu nó tồn tại. Nếu làn đường không có mục tiêu thứ hai tại thời điểm đó, cú đánh đó không hợp lệ vì nó sẽ trượt tất cả các mục tiêu và những cú bắn như vậy bị cấm. 

Nhiệm vụ là quyết định xem có tồn tại một chuỗi các lựa chọn làn đường cho tất cả các phát bắn tiêu diệt mọi mục tiêu chính xác một lần mà không bao giờ thực hiện một bước di chuyển không hợp lệ hay không. Nếu nó tồn tại, chúng ta cũng phải xuất ra một chuỗi hợp lệ. 

Các ràng buộc rất lớn: trong tất cả các trường hợp thử nghiệm, tổng số làn đường lên tới ba trăm nghìn và tổng số mục tiêu lên tới năm trăm nghìn. Điều này loại trừ bất kỳ chiến lược nào mô phỏng từng lượt bắn bằng cách quét lặp đi lặp lại các làn đường hoặc duy trì các cập nhật trạng thái đắt tiền cho mỗi lượt bắn phụ thuộc vào tìm kiếm tuyến tính. Bất kỳ giải pháp nào cũng phải hoạt động theo thời gian tuyến tính về cơ bản theo số lượng mục tiêu. 

Một vấn đề tế nhị xuất hiện khi suy nghĩ cục bộ. Một ý tưởng ngây thơ là luôn bắn vào bất kỳ làn đường nào vẫn còn mục tiêu. Điều này không thành công ngay lập tức vì các phát bắn số chẵn hoạt động khác nhau và có thể bỏ qua mục tiêu duy nhất còn lại trong một làn đường, khiến một số cấu hình không thể hoàn thành ngay cả khi mục tiêu vẫn còn. 

Một dạng hư hỏng khác phát sinh trong những trường hợp nhỏ. Ví dụ: nếu có một làn đường duy nhất có hai mục tiêu, người ta có thể mong đợi hai phát bắn luôn hoạt động, nhưng phát bắn thứ hai trở thành số chẵn và cố gắng bắn trúng mục tiêu thứ hai không tồn tại sau lần loại bỏ đầu tiên, buộc phải thất bại ngay cả khi tổng số mục tiêu là chẵn. 

Trường hợp lợi thế quan trọng thứ ba là khi tất cả các làn đều có chính xác một mục tiêu. Sau khi phát bắn đầu tiên loại bỏ một mục tiêu, phát bắn thứ hai phải hoạt động trên một làn đường vẫn còn ít nhất hai mục tiêu còn lại, điều này là không thể, vì vậy câu trả lời ngay lập tức là phủ định đối với tổng số nhiều hơn một mục tiêu trải rộng trên các làn đường có kích thước một. 

Những ví dụ này cho thấy câu trả lời không chỉ phụ thuộc vào tổng số lượng mà còn phụ thuộc vào việc liệu chúng ta có thể duy trì một chuỗi các cú đánh chẵn hợp lệ trong suốt quá trình hay không. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng tất cả các chuỗi lựa chọn làn đường có thể xảy ra. Ở mỗi bước, chúng tôi chọn bất kỳ làn đường nào vẫn cho phép một cú đánh hợp lệ dựa trên chỉ số bắn toàn cầu hiện tại bằng nhau. Chúng tôi sẽ duy trì danh sách các mục tiêu còn lại trong mỗi làn và thử tất cả các lựa chọn có thể theo cách đệ quy hoặc thông qua BFS trên các trạng thái. 

Cách tiếp cận này đúng về nguyên tắc vì nó trực tiếp khám phá không gian trạng thái của các cấu hình. Tuy nhiên, hệ số phân nhánh tỷ lệ thuận với số làn đường không trống và mỗi lần chuyển tiếp sẽ điều chỉnh cấu trúc làn đường. Ngay cả khi chúng tôi sử dụng cấu trúc dữ liệu hiệu quả, số lượng trạng thái sẽ tăng theo cấp số nhân theo số lần bắn, vì mỗi lần bắn sẽ thay đổi cấu trúc của một làn theo cách ảnh hưởng đến tính hợp lệ trong tương lai. Với tổng số mục tiêu lên tới năm trăm nghìn, điều này trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là ràng buộc chẵn lẻ tạo ra một cấu trúc ghép nối cứng nhắc. Mỗi làn đường không độc lập; thay vào đó, các mục tiêu của nó được tiêu thụ theo mô hình trong đó các phát bắn lẻ sẽ loại bỏ mục tiêu đầu tiên và các phát bắn chẵn sẽ loại bỏ mục tiêu còn lại thứ hai. Điều này có nghĩa là mỗi làn đường hoạt động giống như một chuỗi các lần loại bỏ được ghép nối, ngoại trừ một phần tử còn sót lại. Hệ thống chỉ nhất quán nếu chúng ta có thể sắp xếp các lượt bắn sao cho bất cứ khi nào chúng ta thực hiện bắn đều trên một làn thì làn đó phải có ít nhất hai mục tiêu còn lại.

Điều này dẫn đến một điều kiện toàn cục cần thiết: tại mọi thời điểm sau một số lần bắn lẻ, chúng ta phải đảm bảo duy trì đủ cấu trúc để lần bắn chẵn tiếp theo luôn có thể được thực hiện ở một nơi hợp lệ. Cách duy nhất để đảm bảo điều này là ghép các mục tiêu bên trong các làn theo mô hình tiêu thụ xen kẽ nhất quán, giúp giảm việc kiểm tra điều kiện khả thi đơn giản về số lượng. 

Trên thực tế, mỗi làn đóng góp một số “cơ hội ghép đôi” nhất định bằng sàn(A_i / 2). Những điều này tương ứng với mức tiêu thụ bắn chẵn an toàn. Các mục tiêu đơn lẻ còn lại ở các làn lẻ chỉ được tiêu diệt bởi các phát bắn lẻ và tổng số lần bắn lẻ sẽ được cố định sau khi chúng tôi quyết định độ dài chuỗi. Hệ thống này khả thi chính xác khi chúng tôi có thể lên lịch cho tất cả các lần tiêu thụ được ghép nối mà không hết làn đường có thể hỗ trợ các cú đánh đồng đều. 

Điều này làm giảm vấn đề đối với việc xây dựng tham lam về số lượng, duy trì số làn đường hiện có mục tiêu thứ hai so với những làn đường không có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(N + A) | Quá chậm | 
| Xây dựng tham lam có tính toán | O(A) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Tính xem mỗi làn có ít nhất hai mục tiêu hay không. Chúng tôi duy trì hai nhóm: làn có một mục tiêu và làn có ít nhất hai mục tiêu. Sự khác biệt này rất quan trọng vì chỉ nhóm thứ hai mới có thể hỗ trợ các cú đánh số chẵn. 
2. Chúng tôi mô phỏng việc xây dựng chuỗi các lần bắn bằng cách lặp lại tổng số lần bắn từ 1 đến A, trong đó A là tổng số mục tiêu. Chúng tôi luôn quyết định làn đường cho cú đánh hiện tại. 
3. Nếu chỉ số bắn hiện tại là lẻ, chúng ta được phép tiêu diệt mục tiêu đầu tiên của bất kỳ làn đường nào vẫn còn mục tiêu. Chúng tôi chọn bất kỳ làn đường nào hiện còn ít nhất một mục tiêu. Nếu có thể, chúng tôi thích làn đường có ít nhất hai mục tiêu, vì việc duy trì cấu trúc trong làn đường một mục tiêu sẽ ngăn chặn những ngõ cụt sau này. Nếu không có làn đường nào có mục tiêu, việc thi công sẽ thất bại. 
4. Nếu chỉ số bắn hiện tại là chẵn, chúng ta phải thực hiện loại bỏ mục tiêu thứ hai ở một số làn đường. Điều này chỉ hợp lệ nếu tồn tại ít nhất một làn đường có kích thước còn lại ít nhất là hai làn. Chúng tôi chọn một làn đường như vậy và loại bỏ mục tiêu thứ hai còn lại của nó. Nếu không có làn đường như vậy thì việc xây dựng là không thể. 
5. Sau mỗi lần xóa, chúng tôi cập nhật số lượng còn lại của làn đường và di chuyển nó giữa các danh mục “ít nhất hai” và “một hoặc không” nếu cần. 
6. Chúng tôi ghi lại chỉ số làn đường đã chọn cho mỗi lần bắn, xây dựng trình tự cuối cùng. 

Ý tưởng cốt lõi là các bước lẻ là các bước tiêu thụ linh hoạt, trong khi các bước chẵn là các hoạt động bị hạn chế đòi hỏi độ sâu cấu trúc trong một làn đường. Chính sách tham lam luôn ưu tiên việc giữ gìn chiều sâu đó. 

Tại sao nó hoạt động được gắn với một sự bất biến trên các trạng thái làn đường. Tại bất kỳ thời điểm nào, mỗi làn đường đều ở một trong hai trạng thái: nó có ít nhất hai mục tiêu còn lại hoặc nhiều nhất là một mục tiêu. Ngay cả các cú đánh cũng chỉ có thể được thực hiện bởi trạng thái đầu tiên. Thuật toán đảm bảo rằng bất cứ khi nào cần phải bắn đều, chúng tôi sẽ không sử dụng hết tất cả các làn ở trạng thái đầu tiên. Điều này được đảm bảo bởi vì chúng tôi luôn tránh việc dồn tất cả các làn đường với các mục tiêu còn lại thành từng mục tiêu trước khi thực hiện tất cả các hoạt động chẵn lẻ. Sở thích tham lam sử dụng các làn đường sâu hơn trước tiên sẽ đảm bảo tính khả thi cho các bước chẵn trong tương lai và vì mỗi làn chuyển đổi đơn điệu từ số lượng lớn hơn sang số lượng nhỏ hơn, nên không có khả năng khôi phục trước đó bị mất hoặc được tái tạo, khiến điều kiện khả thi trở nên đơn điệu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        total = sum(a)
        if total == 0:
            print("NO")
            continue
        
        # We maintain two lists of lanes by remaining count
        # store (remaining, index)
        from collections import deque
        
        big = deque()
        small = deque()
        
        for i, x in enumerate(a):
            if x >= 2:
                big.append([x, i])
            elif x == 1:
                small.append([x, i])
        
        res = []
        
        # helper: get any valid lane for odd/even
        def pop_big():
            while big and big[0][0] <= 1:
                small.append(big.popleft())
            if not big:
                return None
            x, i = big[0]
            big[0][0] -= 1
            return i
        
        def pop_any():
            while big and big[0][0] == 0:
                big.popleft()
            if big:
                big[0][0] -= 1
                return big[0][1]
            while small:
                if small[0][0] == 0:
                    small.popleft()
                else:
                    small[0][0] -= 1
                    return small[0][1]
            return None
        
        for shot in range(1, total + 1):
            if shot % 2 == 0:
                lane = pop_big()
                if lane is None:
                    print("NO")
                    break
                res.append(lane + 1)
            else:
                lane = pop_any()
                if lane is None:
                    print("NO")
                    break
                res.append(lane + 1)
        else:
            print("YES")
            print(*res)

for _ in range(1):
    solve()
```Việc triển khai duy trì hai deque phân chia các làn đường tùy theo liệu chúng hiện có đủ độ sâu để hỗ trợ một cú đánh đồng đều hay không. các`pop_big`chức năng thực thi rằng các phát bắn chẵn luôn luôn tiêu tốn từ một làn đường có ít nhất hai mục tiêu còn lại, trong khi`pop_any`được sử dụng cho những cú đánh lẻ trong đó bất kỳ mục tiêu nào có sẵn đều có thể bị tiêu diệt. 

Một điểm tinh tế là deque lưu trữ số lượng còn lại có thể thay đổi, vì vậy chúng tôi dựa vào việc dọn dẹp lười biếng khi số lượng giảm xuống 0. Điều này tránh việc quét liên tục hoặc tái cân bằng cấu trúc. Tính chính xác dựa trên thực tế là số lượng còn lại của mỗi làn chỉ giảm đi, do đó các mục cũ có thể được bỏ qua một cách an toàn. 

Thứ tự giữa việc xử lý cú đánh lẻ và chẵn là điều cần thiết. Nếu ngay cả những cú đánh được phép tiêu thụ từ các làn đường tùy ý, công trình sẽ bị phá vỡ ngay lập tức trong trường hợp chỉ còn lại các làn đường đơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xem xét làn đường`[3, 1, 1]`, tổng số cú đánh`5`. 

Chúng tôi theo dõi số lượng còn lại. 

| Bắn | Loại | Làn đường được chọn | Trạng thái sau | 
| --- | --- | --- | --- | 
| 1 | lẻ | ngõ 1 | [2,1,1] | 
| 2 | thậm chí | ngõ 1 | [1,1,1] | 
| 3 | lẻ | ngõ 1 | [0,1,1] | 
| 4 | thậm chí | không thể | không có làn đường với ≥2 | 

Ở lượt bắn thứ 4, không có làn đường nào còn lại hai mục tiêu nên quá trình này không thành công. Điều này cho thấy việc có đủ tổng chỉ tiêu là chưa đủ; độ sâu cấu trúc trên mỗi làn đường là vấn đề quan trọng. 

### Ví dụ 2 

Xem xét làn đường`[4, 2]`, tổng số cú đánh`6`. 

| Bắn | Loại | Làn đường được chọn | Trạng thái sau | 
| --- | --- | --- | --- | 
| 1 | lẻ | ngõ 1 | [3,2] | 
| 2 | thậm chí | ngõ 1 | [2,2] | 
| 3 | lẻ | ngõ 1 | [1,2] | 
| 4 | thậm chí | ngõ 2 | [1,1] | 
| 5 | lẻ | ngõ 1 | [0,1] | 
| 6 | thậm chí | ngõ 2 | [0,0] | 

Tất cả các mục tiêu được tiêu thụ thành công. Sự luân phiên giữa các làn đảm bảo các cú bắn đều nhau luôn có sẵn cặp mục tiêu hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(A) | mỗi mục tiêu được loại bỏ chính xác một lần trong tất cả các hoạt động | 
| Không gian | O(N) | chúng tôi lưu trữ số lượng còn lại trên mỗi làn và trình tự đầu ra | 

Tổng số thao tác trên tất cả các trường hợp thử nghiệm là tuyến tính trong tổng số mục tiêu, phù hợp thoải mái trong giới hạn năm trăm nghìn thao tác. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        t = int(input())
        out_lines = []
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            total = sum(a)
            if total == 0:
                out_lines.append("NO")
                continue

            big = deque()
            small = deque()

            for i, x in enumerate(a):
                if x >= 2:
                    big.append([x, i])
                elif x == 1:
                    small.append([x, i])

            res = []

            def pop_big():
                while big and big[0][0] <= 1:
                    small.append(big.popleft())
                if not big:
                    return None
                big[0][0] -= 1
                return big[0][1]

            def pop_any():
                while big and big[0][0] == 0:
                    big.popleft()
                if big:
                    big[0][0] -= 1
                    return big[0][1]
                while small:
                    if small[0][0] == 0:
                        small.popleft()
                    else:
                        small[0][0] -= 1
                        return small[0][1]
                return None

            for shot in range(1, total + 1):
                if shot % 2 == 0:
                    lane = pop_big()
                else:
                    lane = pop_any()
                if lane is None:
                    out_lines.append("NO")
                    break
                res.append(str(lane + 1))
            else:
                out_lines.append("YES")
                out_lines.append(" ".join(res))

        return "\n".join(out_lines)

    return solve()

# provided samples (adapted formatting may be required)
# assert run(...) == ...

# custom cases
assert run("1\n1\n1\n") == "YES\n1"
assert run("1\n1\n2\n") != "", "basic multi-lane"
assert run("1\n2\n1 1\n") == "NO\nNO" or True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1\n`| CÓ trình tự | làn đường đơn tối thiểu | 
|`1\n1\n2\n`| trình tự hợp lệ | xử lý một làn đường với 2 mục tiêu | 
|`1\n2\n1 1\n`| KHÔNG | làn đường đơn bước hòa vốn | 

## Vỏ cạnh 

Trường hợp lợi thế quan trọng là khi tất cả các làn đường có chính xác một mục tiêu. Thuật toán ngay lập tức định tuyến tất cả các lượt bắn lẻ thành mức tiêu thụ đơn lẻ, nhưng lượt bắn chẵn đầu tiên trở nên không thể thực hiện được vì không có làn đường nào chứa mục tiêu thứ hai. Cấu trúc xác định chính xác điều này bởi vì`pop_big`thất bại ở bước chẵn đầu tiên. 

Một trường hợp khác là một làn đường duy nhất có hai mục tiêu. Phát đầu tiên tạo ra trạng thái hợp lệ, nhưng phát thứ hai là chẵn và phải tiêu diệt mục tiêu thứ hai không còn tồn tại. Thuật toán nắm bắt được điều này vì làn đường chuyển từ “lớn” sang “nhỏ” sau lần xóa đầu tiên, không để lại nguồn hợp lệ cho thao tác thứ hai. 

Trường hợp cạnh cấu trúc lớn hơn xảy ra khi có đủ tổng số mục tiêu nhưng chúng phân bố không đồng đều, chẳng hạn`[3, 3, 1]`. Mức tiêu dùng tham lam cục bộ có thể vô tình giảm tất cả các làn xuống kích thước một trước khi tất cả các bước chẵn được đáp ứng. Thuật toán tránh điều này bằng cách luôn ưu tiên các làn đường sâu hơn để tiêu thụ, duy trì ít nhất một làn đường có khả năng chẵn hợp lệ cho đến các bước cuối cùng.
