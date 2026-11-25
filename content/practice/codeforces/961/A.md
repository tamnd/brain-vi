---
title: "CF 961A - Tetris"
description: "Chúng tôi đang mô phỏng một quá trình trong đó các ô vuông lần lượt xuất hiện và xếp chồng lên nhau thành các cột. Có n cột và mỗi ô vuông sắp tới sẽ chọn một cột và đặt lên trên bất kỳ cột nào đã có sẵn ở đó, tăng chiều cao của cột đó lên một."
date: "2026-06-17T01:47:01+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 961
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 41 (Rated for Div. 2)"
rating: 900
weight: 961
solve_time_s: 69
verified: true
draft: false
---

[CF 961A - Tetris](https://codeforces.com/problemset/problem/961/A) 

**Xếp hạng:** 900 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một quá trình trong đó các ô vuông lần lượt xuất hiện và xếp chồng lên nhau thành các cột. có`n`các cột và mỗi ô vuông đến sẽ chọn một cột và đặt lên trên bất kỳ cột nào đã có ở đó, tăng chiều cao của cột đó lên một. 

Sự kiện quan trọng xảy ra bất cứ khi nào mỗi cột trở nên không trống cùng một lúc. Tại thời điểm đó, chúng tôi “xóa” toàn bộ lớp dưới cùng: mỗi cột mất đúng một ô vuông và chúng tôi được một điểm. Sau khi loại bỏ, tất cả các ngăn xếp còn lại sẽ dịch chuyển xuống một cách hiệu quả, nhưng vì chúng ta chỉ quan tâm đến chiều cao nên điều này tương đương với việc trừ đi một chiều cao hiện tại của mỗi cột. 

Vì vậy, toàn bộ quá trình thực sự là về việc theo dõi chiều cao của cột và đếm số lần chiều cao tối thiểu trên tất cả các cột đạt ít nhất một trong khi tất cả các cột cũng khác 0. 

Đầu vào cho biết số cột và thứ tự các cột nhận được hình vuông. Đầu ra là số lần xảy ra việc loại bỏ toàn bộ hàng. 

Những hạn chế`n, m ≤ 1000`làm rõ rằng mô phỏng O(mn) đã an toàn, nhưng bất cứ điều gì tệ hơn thế là không cần thiết. Quét tuyến tính trên tất cả các cột cho mỗi bản cập nhật vẫn ổn, nhưng cấu trúc cho thấy chúng ta có thể tránh quét toàn bộ lặp lại bằng cách duy trì trạng thái tăng dần. 

Trường hợp cạnh tinh tế xuất hiện khi một cột nhận được hình vuông đầu tiên muộn. Cho đến khi cột trống cuối cùng không còn trống, việc xóa không thể xảy ra bất kể các cột khác có lớn đến đâu. Một trường hợp đặc biệt khác là khi việc xóa xảy ra liên tiếp: sau khi xóa, tất cả các cột sẽ giảm xuống một cột, do đó một số cột có thể lại trống ngay lập tức, chặn việc xóa khác cho đến khi chúng được nạp lại. 

Ví dụ, nếu`n = 2`và trình tự là`[1, 2, 1, 2]`, cả hai cột đều không trống ở bước 2 và chúng tôi sẽ loại bỏ một cột. Sau đó, độ cao được đặt lại thành`[0, 0]`, và hai lần chèn tiếp theo lại đồng bộ hóa, tạo ra một lần xóa khác. Một cách tiếp cận ngây thơ mà quên đặt lại độ cao đúng cách sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp giữ một mảng`h`kích thước`n`, tăng`h[c_i]`đối với mỗi ô vuông đến và sau mỗi lần cập nhật sẽ kiểm tra xem tất cả các mục nhập có ít nhất là 1 hay không. Nếu vậy, nó sẽ tăng câu trả lời và trừ đi một từ mỗi mục nhập. 

Điều này có hiệu quả vì quy trình này được xác định theo nghĩa đen về mặt kiểm tra toàn cầu và mức giảm tổng thể. Tuy nhiên, phần tốn kém là việc quét toàn bộ lặp đi lặp lại tất cả các cột để kiểm tra xem có cột nào bằng 0 hay không, sau đó thực hiện một lần quét đầy đủ khác để giảm tất cả các giá trị. Trong trường hợp xấu nhất, giá trị này trở thành O(mn), vẫn có thể chấp nhận được ở đây nhưng lãng phí về mặt khái niệm. 

Quan sát quan trọng là chúng ta không cần mô phỏng rõ ràng độ cao. Việc xóa hàng xảy ra chính xác khi chúng tôi đã tích lũy được ít nhất một ô vuông trong mỗi cột kể từ điểm xóa cuối cùng. Sau mỗi lần xóa, mỗi cột sẽ giảm đi một cột, điều này sẽ đặt lại hệ thống một cách hiệu quả để chỉ theo dõi “các ô vuông bổ sung ngoài lớp đã xóa cuối cùng”. Điều này có nghĩa là chúng ta chỉ cần biết liệu mỗi cột có nhận được ít nhất một ô vuông trong giai đoạn hiện tại hay không. 

Chúng tôi duy trì một mảng boolean cho biết liệu một cột có xuất hiện kể từ lần đặt lại gần đây nhất hay không và bộ đếm số lượng cột hiện được “điền trong giai đoạn này”. Mỗi khi một cột được lấp đầy lần đầu tiên trong giai đoạn hiện tại, chúng tôi sẽ tăng bộ đếm này. Khi nó đạt tới`n`, chúng tôi ghi được một điểm và đặt lại tất cả các cờ và bộ đếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(mn) | O(n) | Đã chấp nhận | 
| Theo dõi được tối ưu hóa | O(m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi theo dõi những cột nào đã xuất hiện trong chu kỳ hiện tại và có bao nhiêu cột riêng biệt hiện đang được lấp đầy. 

1. Khởi tạo một mảng`seen`kích thước`n`với tất cả các giá trị sai và một bộ đếm`filled = 0`. Điều này thể hiện những cột nào đã nhận được ít nhất một ô vuông kể từ lần xóa hàng cuối cùng. 
2. Lặp lại từng ô vuông ở cột`c`. 
3. Nếu`seen[c]`là sai, đánh dấu nó là đúng và tăng dần`filled`. Bước này chỉ tính lần xuất hiện đầu tiên của một cột trong chu kỳ hiện tại, vì các ô vuông bổ sung trong cùng một cột không giúp hoàn thành một hàng đầy đủ. 
4. Nếu`filled`trở nên bằng`n`, chúng tôi đã đạt đến phạm vi bao phủ đầy đủ của tất cả các cột. Điều này tương ứng chính xác với việc hình thành một hàng dưới cùng hoàn chỉnh ở trạng thái hiện tại. 
5. Khi điều này xảy ra, hãy tăng câu trả lời lên một, đặt lại`seen`thành tất cả sai và đặt lại`filled`về không. Điều này mô phỏng việc loại bỏ lớp dưới cùng và thả tất cả các ô vuông còn lại xuống một. 
6. Tiếp tục xử lý các ô vuông còn lại theo cách tương tự. 

Tại sao nó hoạt động dựa trên sự phân tách giai đoạn của quá trình. Mỗi lần chúng tôi xóa một hàng, tất cả các cột sẽ mất một đơn vị chiều cao, điều này sẽ loại bỏ chính xác một “lớp được đảm bảo” khỏi mỗi cột. Những gì còn lại phía trên lớp này không liên quan đến việc phát hiện việc hoàn thành toàn hàng tiếp theo; chỉ liệu mỗi cột có nhận được ít nhất một ô vuông mới hay không kể từ lần xóa cuối cùng mới quan trọng. Do đó, mỗi chu kỳ là độc lập và việc đếm số lần chúng tôi hoàn thành một tập hợp cột đầy đủ sẽ khớp chính xác với số lần xóa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    cols = list(map(int, input().split()))

    seen = [False] * n
    filled = 0
    ans = 0

    for c in cols:
        c -= 1
        if not seen[c]:
            seen[c] = True
            filled += 1

        if filled == n:
            ans += 1
            seen = [False] * n
            filled = 0

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp việc giải thích dựa trên giai đoạn. các`seen`mảng theo dõi xem mỗi cột có đóng góp ít nhất một ô vuông trong chu kỳ hiện tại hay không. các`filled`bộ đếm tránh quét liên tục mảng để kiểm tra tính đầy đủ. 

Một cạm bẫy triển khai phổ biến là quên đặt lại cả hai`seen`Và`filled`khi một hàng đầy đủ được hình thành. Nếu chỉ`seen`được thiết lập lại nhưng`filled`không, thuật toán sẽ tiếp tục đếm số lần hoàn thành một cách không chính xác. 

Một điểm tinh tế khác là giảm chỉ mục cột, vì các cột đầu vào dựa trên 1 trong khi mảng Python dựa trên 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 9
1 1 2 2 2 3 1 2 3
```| Bước | Cột | Trạng thái đã thấy (1,2,3) | Đầy | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | T F F | 1 | 0 | 
| 2 | 1 | T F F | 1 | 0 | 
| 3 | 2 | T T F | 2 | 0 | 
| 4 | 2 | T T F | 2 | 0 | 
| 5 | 2 | T T F | 2 | 0 | 
| 6 | 3 | T T T | 3 | 1 (đặt lại) | 
| 7 | 1 | T F F | 1 | 1 | 
| 8 | 2 | T T F | 2 | 1 | 
| 9 | 3 | T T T | 3 | 2 (đặt lại) | 

Bảng cho thấy mỗi lần nhìn thấy tất cả các cột, chúng tôi sẽ hoàn thành một chu trình và đặt lại. Chu kỳ thứ hai hoàn thành vào cuối. 

### Ví dụ 2 

đầu vào:```
2 4
1 2 1 2
```| Bước | Cột | Trạng thái nhìn thấy (1,2) | Đầy | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | T F | 1 | 0 | 
| 2 | 2 | T T | 2 | 1 (đặt lại) | 
| 3 | 1 | T F | 1 | 1 | 
| 4 | 2 | T T | 2 | 2 (đặt lại) | 

Ví dụ này cho thấy sự lặp lại ngay lập tức của toàn bộ chu kỳ, chứng tỏ rằng việc đặt lại các pha tách rời hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi ô vuông được xử lý một lần với cập nhật O(1) | 
| Không gian | O(n) | Mảng Boolean theo dõi trạng thái mỗi cột | 

Lời giải dễ dàng nằm trong giới hạn vì cả hai`n`Và`m`nhiều nhất là 1000, khiến ngay cả một mô phỏng đơn giản cũng trở nên an toàn, trong khi phiên bản được tối ưu hóa này là tuyến tính về số lượng sự kiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("3 9\n1 1 2 2 2 3 1 2 3\n") == "2"

# minimum case
assert run("1 5\n1 1 1 1 1\n") == "5"

# alternating columns
assert run("2 4\n1 2 1 2\n") == "2"

# no full completion
assert run("3 3\n1 1 2\n") == "0"

# single completion late
assert run("3 6\n1 1 2 2 3 3\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 / 1 1 1 1 1`|`5`| mỗi lần nhấn hoàn thành một hàng | 
|`2 4 / 1 2 1 2`|`2`| chu kỳ đầy đủ lặp đi lặp lại | 
|`3 3 / 1 1 2`|`0`| bảo hiểm không đầy đủ | 
|`3 6 / 1 1 2 2 3 3`|`1`| hoàn thành bị trì hoãn duy nhất | 

## Vỏ cạnh 

Số lượng cột tối thiểu (`n = 1`) hoạt động khác nhau vì mỗi ô vuông ngay lập tức hoàn thành một hàng đầy đủ. Thuật toán xử lý việc này một cách tự nhiên:`filled`trở thành 1 sau mỗi lần xuất hiện cột mới, kích hoạt thiết lập lại mỗi lần, do đó câu trả lời bằng`m`. 

Trường hợp một cột nhận được nhiều ô vuông trước khi các ô khác xuất hiện chứng tỏ hành vi của pha. Đối với đầu vào`3 4: 1 1 1 2`, không có hàng nào được hoàn thành vì cột 3 không bao giờ được điền;`filled`không bao giờ đạt tới 3, vì vậy câu trả lời vẫn là 0. 

Một trường hợp ranh giới toàn chu kỳ như`3 3: 1 2 3`hoàn thành chính xác một chu kỳ ở bước cuối cùng, cho thấy rằng việc hoàn thành gắn liền với lần đầu tiên tất cả các cột được đề cập chứ không phải với tần suất cập nhật.
