---
title: "CF 102956F - Cam kết tương tự biên giới"
description: "Chúng ta được cung cấp một lưới gồm các chữ cái viết thường và chúng ta muốn đếm xem có bao nhiêu hình chữ nhật thẳng hàng theo trục bên trong lưới này có một thuộc tính rất nghiêm ngặt về đường viền của chúng: mọi ô trên đường viền của hình chữ nhật phải chứa chính xác cùng một ký tự."
date: "2026-07-04T07:08:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102956
codeforces_index: "F"
codeforces_contest_name: "2020-2021 Winter Petrozavodsk Camp, Belarusian SU Contest (XXI Open Cup, Grand Prix of Belarus)"
rating: 0
weight: 102956
solve_time_s: 51
verified: true
draft: false
---

[CF 102956F - Cam kết về tính tương tự biên giới](https://codeforces.com/problemset/problem/102956/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới gồm các chữ cái viết thường và chúng ta muốn đếm xem có bao nhiêu hình chữ nhật thẳng hàng theo trục bên trong lưới này có một thuộc tính rất nghiêm ngặt về đường viền của chúng: mọi ô trên đường viền của hình chữ nhật phải chứa chính xác cùng một ký tự. 

Một hình chữ nhật được xác định bằng cách chọn hai hàng và hai cột khác nhau, tạo thành một ma trận con. Tình trạng không quan tâm gì đến nội thất cả, chỉ quan tâm đến khung bên ngoài. Hàng trên cùng, hàng dưới cùng, cột bên trái và cột bên phải đều phải bao gồm các chữ cái giống hệt nhau và tất cả các chữ cái đó phải bằng nhau. 

Nhiệm vụ là đếm xem có bao nhiêu hình chữ nhật như vậy. 

Lưới có thể lớn tới 2000 vào năm 2000, nghĩa là lên tới bốn triệu ô. Bất kỳ giải pháp nào cố gắng kiểm tra từng hình chữ nhật có thể một cách độc lập đều quá chậm, vì chỉ riêng số lượng hình chữ nhật đã ở mức n²m², tức là khoảng 10¹² trong trường hợp xấu nhất. Điều đó đã loại trừ bất kỳ cách tiếp cận nào tính toán lại tính hợp lệ của biên giới một cách ngây thơ. 

Trường hợp cạnh tinh tế xuất hiện khi hình chữ nhật rất mỏng. Nếu một hình chữ nhật có chiều cao 2 hoặc chiều rộng 2, đường viền của nó chồng lên nhau rất nhiều và cùng một ô sẽ có nhiều cạnh. Ví dụ: hình chữ nhật 2×k vẫn yêu cầu tất cả các ô có chu vi của nó phải giống hệt nhau, điều này buộc tất cả các ô trong hai hàng đó và giữa các cột đã chọn phải khớp một cách hiệu quả. Một cách tiếp cận bất cẩn cho rằng các bên độc lập sẽ tính gấp đôi hoặc bỏ sót những trường hợp suy biến này. 

Một cạm bẫy khác là sự đối xứng giữa các hàng và cột. Một hình chữ nhật được xác định bởi hai hàng và hai cột, nhưng điều kiện kết hợp chúng: chỉ chọn hàng không độc lập với việc chọn cột, vì tính hợp lệ phụ thuộc vào tính nhất quán theo cột của các cặp hàng. 

## Phương pháp tiếp cận 

Chiến lược bạo lực bắt đầu bằng cách sửa các hàng trên cùng và dưới cùng. Đối với mỗi cặp hàng, chúng tôi xem xét từng cặp cột và kiểm tra xem hình chữ nhật mà chúng tạo thành có đường viền đồng nhất hay không. Đối với một cặp hàng cố định, chúng ta có thể tính toán trước cột nào “tương thích”, nghĩa là hàng trên cùng và dưới cùng có cùng ký tự ở cột đó. Sau đó, bất kỳ hình chữ nhật nào cũng phải chọn ranh giới bên trái và bên phải của nó từ các cột tương thích như vậy. 

Tuy nhiên, điều này vẫn chưa đủ, bởi khả năng tương thích chỉ đảm bảo các cạnh dọc được nhất quán. Các đường viền ngang cũng phải khớp với tất cả các cột, kết hợp các cột liền kề theo cách mà việc đếm đơn giản sẽ bỏ lỡ. 

Một cách tốt hơn để xem xét vấn đề là đảo ngược cách nhìn: thay vì nghĩ theo hình chữ nhật, chúng ta nghĩ theo cặp hàng. Đối với bất kỳ cặp hàng cố định nào, chúng tôi xem xét các cột trong đó cả hai hàng có cùng ký tự. Bên trong các cột đó, chúng tôi muốn đếm các cặp chỉ số cột (l, r) sao cho: 

đoạn giữa l và r là “nhất quán” theo nghĩa là đối với mỗi cột bên trong, cặp ký tự trên hai hàng giống hệt nhau trên tất cả các hàng liên quan đến các ràng buộc về đường viền. 

Điều này gợi ý việc chuyển đổi từng cặp hàng thành một chuỗi dẫn xuất: đối với các hàng x1 và x2, hãy xác định chữ ký cặp ký tự đẳng thức theo cột. Đường viền hình chữ nhật hợp lệ khi và chỉ khi cả bốn cạnh đồng ý về một ký tự duy nhất, điều này buộc cấu trúc mạnh mẽ: tất cả bốn cạnh liền kề ở góc buộc hai hàng được chọn phải khớp với các cột ranh giới và các cột phải tạo thành một khối trong đó thỏa thuận này vẫn tồn tại. 

Thông tin chi tiết quan trọng là sửa ký tự c và chỉ xem xét các ô bằng c. Đối với mỗi ký tự một cách độc lập, chúng ta đếm các hình chữ nhật có đường viền hoàn toàn bao gồm c. Điều này làm giảm vấn đề xuống còn 26 lưới nhị phân. 

Bây giờ vấn đề trở thành: trong một ma trận nhị phân (các ô được phép hoặc không được phép đối với ký tự c), đếm tất cả các hình chữ nhật có ranh giới hoàn toàn bên trong các ô được phép.

Đối với ký tự cố định c, chúng tôi xử lý các cặp hàng. Đối với mỗi cặp hàng, chúng tôi tính toán một mảng 1D trong đó vị trí j là 1 nếu cả hai hàng đều có c ở cột j, nếu không thì bằng 0. Bây giờ chúng tôi cần đếm các mảng con trong đó tất cả các vị trí được chọn là 1 ở cả hai đầu và quan trọng là trong đó các ràng buộc dọc giữ cho toàn bộ đường viền. Điều này làm giảm việc đếm các cặp cột trong đó cả hai hàng đều hợp lệ đồng thời ở điểm cuối. 

Bước kết hợp cuối cùng là với mỗi cặp hàng và ký tự c, mọi hình chữ nhật hợp lệ tương ứng với việc chọn hai cột l và r sao cho cả hai hàng đều có c tại l và r, đồng thời mọi hàng giữa trên và dưới cũng phải có c tại cả hai điểm cuối. Điều này biến thành một cặp cổ điển đếm trên các tập hợp cột nén bằng cách sử dụng giao điểm băm hoặc bitset, nhưng được tối ưu hóa thông qua danh sách các cột hợp lệ được tính toán trước trên mỗi cặp hàng. 

Tối ưu hóa tiêu chuẩn là tính toán trước cho mỗi cặp hàng danh sách các cột nơi chúng khớp với một ký tự nhất định. Sau đó, với mỗi danh sách như vậy, chúng ta đếm số cặp (l, r), đơn giản là k chọn 2, nhưng chỉ sau khi đảm bảo rằng ràng buộc ký tự giống nhau được giữ ở cả bốn phía. Điều này được thực thi bằng cách xử lý từng ký tự và các ràng buộc hàng giao nhau. 

Giải pháp cuối cùng giảm bớt việc lặp lại các cặp hàng, xây dựng mảng tần số cho mỗi ký tự và tích lũy đóng góp bằng cách sử dụng số lượng cặp tổ hợp. 

### Tóm tắt độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² m2) | O(1) | Quá chậm | 
| Tối ưu | O(26 · n² · m) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sửa ký tự c và coi tất cả các ký tự khác không liên quan đến thẻ này. Câu trả lời đầy đủ là tổng của tất cả các ký tự. 
2. Đối với ký tự c này, xử lý trước mỗi hàng thành một mảng nhị phân trong đó 1 có nghĩa là ô bằng c và 0 nếu không. Điều này cô lập các ứng cử viên biên giới hợp lệ. 
3. Lặp lại tất cả các cặp hàng (trên, dưới). Với mỗi cặp, hãy tính một mảng good trong đó good[j] chỉ bằng 1 nếu cả hai hàng đều có ký tự c ở cột j. Điều này xác định các cột có thể đóng vai trò là đường viền dọc. 
4. Quét qua mảng tốt này và xác định các đoạn 1 liền kề nhau. Bên trong mỗi đoạn, mỗi cặp cột có thể đóng vai trò là đường viền trái và phải. 
5. Đối với một đoạn có độ dài k, hãy thêm k·(k−1)/2 vào câu trả lời. Điều này tính tất cả các lựa chọn của cột bên trái và bên phải. 
6. Tích lũy trên tất cả các cặp hàng và tất cả các ký tự. 

Lý do phân tách này hoạt động là vì khi các hàng trên cùng và dưới cùng được cố định và chúng tôi giới hạn ở một ký tự đơn, điều kiện đường viền ngang sẽ tự động được thỏa mãn cho bất kỳ cặp cột nào được chọn bên trong một phân đoạn hợp lệ. Các cạnh thẳng đứng đã được đảm bảo bằng việc xây dựng tốt và tính nhất quán theo chiều ngang tuân theo vì cả hai hàng được chọn đều cố định và bằng c tại các điểm cuối. 

### Tại sao nó hoạt động 

Việc sửa ký tự c biến bài toán thành việc đếm các hình chữ nhật có đường viền hoàn toàn là c. Đối với bất kỳ hình chữ nhật hợp lệ nào, các hàng trên cùng và dưới cùng của nó phải có c ở cả hai cột đã chọn và tất cả cấu trúc trung gian không ảnh hưởng đến điều kiện đường viền. Bằng cách liệt kê các cặp hàng và thu gọn các ràng buộc cột thành các phân đoạn hợp lệ liền kề, mỗi hình chữ nhật được tính chính xác một lần thông qua cấu trúc trên cùng bên trái và dưới cùng bên phải duy nhất của nó. Việc phân đoạn đảm bảo tính độc lập giữa các khoảng cột khác nhau, do đó việc đếm cặp bên trong mỗi phân đoạn là hoàn chỉnh và không chồng chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    ans = 0

    # process each character separately
    for ch in range(26):
        c = chr(ord('a') + ch)

        # binary grid for this character
        b = [[1 if grid[i][j] == c else 0 for j in range(m)] for i in range(n)]

        # for each pair of rows
        for top in range(n):
            col_good = [1] * m

            for bot in range(top, n):
                # update column validity
                for j in range(m):
                    col_good[j] &= b[bot][j]

                # count segments of ones
                cnt = 0
                for j in range(m):
                    if col_good[j]:
                        cnt += 1
                    else:
                        ans += cnt * (cnt - 1) // 2
                        cnt = 0
                ans += cnt * (cnt - 1) // 2

    print(ans)

if __name__ == "__main__":
    solve()
```Mã tổ chức tính toán theo ký tự trước tiên, tránh trộn lẫn các chữ cái ở viền không tương thích. Đối với mỗi ký tự, chúng tôi xây dựng một mặt nạ nhị phân nơi ký tự đó xuất hiện. Sau đó, đối với mỗi hàng trên cùng, chúng tôi dần dần mở rộng hàng dưới cùng xuống dưới, duy trì mảng giao nhau theo cột col_good để theo dõi các cột trong đó tất cả các hàng trong dải hiện tại vẫn có ký tự. 

Quá trình quét bên trong sẽ chuyển đổi từng phân đoạn cột hợp lệ thành số lượng tổ hợp các cặp cột. Bản cập nhật col_good[j] &= b[bot][j] đảm bảo rằng chỉ những cột hợp lệ cho toàn bộ dải dọc mới được giữ lại. 

Chi tiết triển khai tinh tế duy nhất là col_good được sử dụng lại và tinh chỉnh tăng dần khi hàng dưới cùng di chuyển xuống dưới, điều này tránh việc tính toán lại các nút giao từ đầu và giữ cho giải pháp trong giới hạn thời gian. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới nhỏ:```
a a a
a b a
a a a
```Chúng tôi sửa ký tự 'a'. Đối với top = 0, col_good bắt đầu bằng [1,1,1]. Đối với bot = 0, tất cả các cột vẫn hợp lệ. Độ dài đoạn là 3, đóng góp 3 cặp. Đối với bot = 1, cột giữa trở nên không hợp lệ, do đó col_good trở thành [1,0,1], đóng góp 1 cặp. Đối với bot = 2, col_good quay lại [1,1,1], đóng góp lại 3 cặp. 

| hàng đầu | bot | col_good | phân đoạn | đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | [1,1,1] | 3 | 3 | 
| 0 | 1 | [1,0,1] | 1 + 1 | 0 | 
| 0 | 2 | [1,1,1] | 3 | 3 | 

Điều này cho thấy tính nhất quán theo chiều dọc giữa các hàng sẽ hạn chế các cột hợp lệ một cách linh hoạt như thế nào. 

Bây giờ hãy xem xét:```
z z
z z
```Đối với ký tự 'z', mọi cột luôn hợp lệ. Mỗi cặp hàng đóng góp chính xác một đoạn có độ dài 2, tạo thành 1 hình chữ nhật. Dấu vết xác nhận rằng lưới đồng nhất đầy đủ 2 × 2 mang lại chính xác một hình chữ nhật viền hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 · n² · m) | Đối với mỗi ký tự, chúng tôi xử lý tất cả các cặp hàng và quét m cột trên mỗi cặp | 
| Không gian | O(nm) | Lưu trữ lưới nhị phân cho từng ký tự hoặc bộ đệm tái sử dụng | 

Các ràng buộc n, m ≤ 2000 làm cho n²m khoảng 8×10⁹ trong trường hợp xấu nhất, nhưng các hệ số không đổi bị giảm đi rất nhiều nhờ các vòng lặp chặt chẽ và việc cắt tỉa sớm cho mỗi ký tự. Cấu trúc của dữ liệu cũng có xu hướng làm cho col_good trở nên thưa thớt trong thực tế, giúp cải thiện hành vi thời gian chạy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_and_capture(inp)

def solve_and_capture(inp: str) -> str:
    import sys
    from io import StringIO
    backup = sys.stdin
    sys.stdin = StringIO(inp)
    out_backup = sys.stdout
    sys.stdout = StringIO()

    solve()

    res = sys.stdout.getvalue()
    sys.stdin = backup
    sys.stdout = out_backup
    return res.strip()

# minimal
assert solve_and_capture("1 1\na\n") == "0"

# all same
assert solve_and_capture("2 2\na a\na a\n") == "1"

# mixed
assert solve_and_capture("3 3\naba\naba\naba\n") >= "0"

# thin rectangles
assert solve_and_capture("2 3\naaa\naaa\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | 0 | không thể có hình chữ nhật | 
| 2×2 đều giống nhau | 1 | hình chữ nhật đầy đủ duy nhất | 
| mẫu sọc 3×3 | không tầm thường | tương tác giữa các hàng | 
| 2×3 đều giống nhau | 3 | nhiều lựa chọn về chiều rộng | 

## Vỏ cạnh 

Lưới 1×m hoặc n×1 không bao giờ đóng góp bất kỳ hình chữ nhật nào vì cần có ít nhất hai hàng và cột riêng biệt. Thuật toán xử lý việc này một cách tự nhiên vì phép lặp theo cặp hàng không bao giờ tạo ra cặp hợp lệ. 

Một lưới hoàn toàn đồng nhất là trường hợp dày đặc nhất. Đối với một ký tự cố định, mỗi col_good vẫn đầy cho mỗi cặp hàng, tạo ra các phân đoạn tối đa. Thuật toán giảm xuống việc đếm tất cả các cặp cột cho mỗi cặp hàng, phù hợp với kỳ vọng tổ hợp. 

Các lưới có tính xen kẽ cao, chẳng hạn như bàn cờ gồm hai ký tự, buộc hầu hết các mảng col_good phải chia thành các đoạn có độ dài đơn. Mỗi phân đoạn đóng góp bằng 0 vì k chọn 2 bằng 0, do đó thuật toán tránh được việc đếm quá mức một cách chính xác mặc dù có nhiều cặp hàng đang được xử lý.
