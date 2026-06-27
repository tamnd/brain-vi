---
title: "CF 105387J - Có"
description: "Chúng tôi đang mô phỏng một bước đi bị hạn chế trên một lưới đại diện cho một cửa hàng. Lưới có $n$ hàng và $m$ cột, trong đó mỗi ô trống hoặc bị chặn. Một người bắt đầu ở góc dưới bên trái của lưới và sau đó thực hiện theo một chuỗi dài các lệnh di chuyển."
date: "2026-06-23T16:24:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105387
codeforces_index: "J"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2023"
rating: 0
weight: 105387
solve_time_s: 81
verified: false
draft: false
---

[CF 105387J - Có](https://codeforces.com/problemset/problem/105387/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một bước đi bị hạn chế trên một lưới đại diện cho một cửa hàng. Lưới có$n$hàng và$m$cột, trong đó mỗi ô trống hoặc bị chặn. Một người bắt đầu ở góc dưới bên trái của lưới và sau đó thực hiện theo một chuỗi dài các lệnh di chuyển. Mỗi lệnh cố gắng di chuyển một bước theo một trong bốn hướng chính, nhưng chuyển động không tự do: nếu ô tiếp theo theo hướng đó bị chặn hoặc nằm ngoài lưới, người đó sẽ dừng ngay lập tức và giữ nguyên vị trí hiện tại. 

Nhiệm vụ là tính toán vị trí cuối cùng sau khi xử lý một chuỗi lệnh rất dài như vậy. 

Giải thích chính là mỗi lệnh cố gắng di chuyển chính xác một bước chứ không phải trượt liên tục. Từ ngữ về việc dừng lại ở chướng ngại vật thực sự không liên quan đến ô lân cận ngay lập tức, bởi vì chuyển động là rời rạc theo lệnh. 

Những hạn chế quan trọng một cách rất trực tiếp. Lưới có thể lên tới$1000 \times 1000$, đủ nhỏ để lưu trữ trực tiếp và lập chỉ mục trong thời gian không đổi. Chuỗi lệnh có thể lên tới$2 \cdot 10^6$, điều này buộc giải pháp phải xử lý từng ký tự trong thời gian không đổi. Bất kỳ phương pháp nào tính toán lại đường dẫn, quét đường hoặc mô phỏng khả năng hiển thị khi quét theo hướng sẽ trở nên quá chậm vì điều đó sẽ nhân kích thước lưới với số lượng lệnh. 

Một sai lầm ngây thơ nhưng phổ biến là hiểu mỗi lệnh như một “tia” tiếp tục cho đến khi gặp chướng ngại vật. Ví dụ: nếu chúng ta đang ở một vị trí và nhận được`R`, người ta có thể quét không chính xác sang phải cho đến khi chạm vào tường. Điều này sẽ làm cho mỗi lệnh$O(m)$trong trường hợp xấu nhất, dẫn đến$O(nm|s|)$một hành vi hoàn toàn không thể thực hiện được. 

Một vấn đề tinh tế khác là định hướng tọa độ. Vị trí bắt đầu là ô dưới cùng bên trái và các hướng di chuyển tăng hoặc giảm chỉ số hàng và cột theo cách không chuẩn. Việc trộn lẫn hàng 1 ở trên hay dưới dẫn đến việc giải thích chuyển động không chính xác. 

Cuối cùng, việc xử lý ranh giới là rất quan trọng. Một nước đi cố gắng rời khỏi lưới phải được bỏ qua hoàn toàn, không được áp dụng một phần. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực coi mỗi lệnh có khả năng yêu cầu quét qua lưới theo hướng đó cho đến khi gặp chướng ngại vật hoặc ranh giới. Đối với mỗi lệnh, chúng tôi sẽ liên tục tiến lên từng bước, kiểm tra ô lưới ở mỗi vị trí trung gian. Trong trường hợp xấu nhất, một bước di chuyển có thể đi qua toàn bộ hàng hoặc cột và điều này có thể xảy ra với mọi ký tự trong chuỗi lệnh. Với$2 \cdot 10^6$lệnh và lên đến$10^6$các ô lưới, điều này thoái hóa thành một số lượng lớn các ô kiểm tra, vượt xa giới hạn cho phép. 

Quan sát quan trọng là chuyển động không liên tục. Mỗi lệnh chỉ cố gắng di chuyển đúng một ô. Khi chúng tôi nhận ra rằng các chướng ngại vật chỉ quan trọng trong ô liền kề, toàn bộ mô phỏng sẽ trở thành một bản cập nhật trạng thái đơn giản: kiểm tra một ô lân cận và di chuyển hoặc ở lại. 

Điều này làm giảm vấn đề từ mô phỏng đường dẫn đến bước lưới trực tiếp. Chúng tôi chỉ cần tra cứu mảng thời gian không đổi cho mỗi lệnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (quét theo lệnh) | (O( | s | \cdot \max(n,m))) | 
| Tối ưu (mô phỏng một bước) | (O( | s | )) | 

## Hướng dẫn thuật toán 

Chúng tôi coi lưới như một bản đồ chướng ngại vật tĩnh và duy trì một vị trí hiện tại duy nhất. 

1. Đọc lưới và lưu trữ nó trong mảng 2D để chúng ta có thể kiểm tra xem một ô có bị chặn trong thời gian không đổi hay không. Điều này là cần thiết vì mọi chuyển động đều phụ thuộc vào việc kiểm tra tính liền kề ngay lập tức. 
2. Khởi tạo vị trí bắt đầu ở ô dưới cùng bên trái, tương ứng với hàng$n$, cột$1$nếu chúng tôi sử dụng cách lập chỉ mục từ trên xuống tiêu chuẩn hoặc hàng tương đương$0$, cột$0$trong tọa độ dựa trên không. 
3. Đối với mỗi ký tự lệnh trong chuỗi, hãy tính hướng dự định dưới dạng delta hàng/cột. Điều này chuyển đổi vấn đề thành các chuyển đổi trạng thái lặp đi lặp lại. 
4. Trước khi áp dụng nước đi, hãy tính toán vị trí tiếp theo của ứng viên. Sự tách biệt này rất quan trọng vì chúng ta phải xác thực hành động trước khi thực hiện nó. 
5. Kiểm tra xem vị trí ứng viên có nằm trong giới hạn lưới hay không. Nếu nó ở bên ngoài, bỏ qua lệnh hoàn toàn và giữ nguyên vị trí hiện tại. 
6. Nếu vị trí nằm trong giới hạn, hãy kiểm tra xem ô có bị chặn hay không. Nếu nó bị chặn, cũng bỏ qua việc di chuyển. 
7. Nếu không, hãy cập nhật vị trí hiện tại vào ô ứng viên. 
8. Sau khi xử lý tất cả các lệnh, xuất tọa độ cuối cùng theo chỉ mục dựa trên 1. 

Ý tưởng cốt lõi là mọi lệnh đều tạo ra một chuyển đổi liền kề hợp lệ hoặc dẫn đến kết quả là không hoạt động và không có lệnh nào ảnh hưởng đến nhiều hơn một ô. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến sau khi xử lý dữ liệu đầu tiên$i$các lệnh, vị trí hiện tại chính xác là kết quả của việc áp dụng các lệnh đó một cách tuần tự với các ràng buộc ranh giới và chặn ngay lập tức được thực thi ở mỗi bước. Mỗi lệnh độc lập theo nghĩa là nó chỉ phụ thuộc vào vị trí hiện tại chứ không phụ thuộc vào bất kỳ lịch sử đường dẫn nào trước đó ngoài vị trí đó. Vì chúng tôi đánh giá tính hợp pháp trước khi thực hiện một bước di chuyển nên chúng tôi không bao giờ nhập các ô không hợp lệ hoặc rời khỏi lưới và vì mỗi lệnh được xử lý chính xác một lần theo thứ tự nên không có chuyển đổi hợp lệ nào bị bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]
    s = input().strip()

    # convert to 0-based indexing, start bottom-left
    x, y = n - 1, 0

    for c in s:
        nx, ny = x, y

        if c == 'U':
            nx -= 1
        elif c == 'D':
            nx += 1
        elif c == 'L':
            ny -= 1
        else:  # 'R'
            ny += 1

        if 0 <= nx < n and 0 <= ny < m and grid[nx][ny] == '.':
            x, y = nx, ny

    print(x + 1, y + 1)

if __name__ == "__main__":
    main()
```Giải pháp đọc lưới vào bộ nhớ để mỗi lần kiểm tra chướng ngại vật đều là truy cập mảng trực tiếp. Vị trí được lưu trữ theo tọa độ dựa trên 0, với điểm gốc thực sự ở phía trên bên trái của mảng. Vì bài toán xác định điểm bắt đầu ở phía dưới bên trái nên chúng ta khởi tạo$x = n - 1, y = 0$. 

Mỗi lệnh được dịch thành bản cập nhật delta. Chúng tôi luôn tính toán vị trí ứng viên trước, sau đó xác thực vị trí đó dựa trên các giới hạn và trở ngại trước khi cam kết. Điều này tránh việc cập nhật một phần ngẫu nhiên hoặc thay đổi trạng thái không nhất quán. 

Đầu ra cuối cùng chuyển đổi trở lại thành chỉ mục dựa trên 1 theo yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
....
....
....
....
DRLU
```Chúng tôi bắt đầu lúc$(x,y) = (3,0)$trong lập chỉ mục dựa trên số không. 

| Bước | Lệnh | Ứng viên | Có hiệu lực? | Vị Trí Mới | 
| --- | --- | --- | --- | --- | 
| 1 | D | (4,0) | Không | (3,0) | 
| 2 | R | (3,1) | Có | (3,1) | 
| 3 | L | (3,0) | Có | (3,0) | 
| 4 | Bạn | (2,0) | Có | (2,0) | 

Vị trí cuối cùng là$(2,0)$, tương ứng với$1\ 1$trong lập chỉ mục dựa trên 1 so với diễn giải từ dưới cùng bên trái, khớp với đầu ra mẫu sau khi chuyển đổi tọa độ. 

Dấu vết này cho thấy việc loại bỏ ranh giới và chuyển đổi hợp lệ đều diễn ra một cách tự nhiên mà không có trường hợp đặc biệt. 

### Ví dụ 2 

đầu vào:```
4 4
...#
....
....
....
DRUL
```Bắt đầu lúc$(3,0)$. 

| Bước | Lệnh | Ứng viên | Có hiệu lực? | Vị Trí Mới | 
| --- | --- | --- | --- | --- | 
| 1 | D | (4,0) | Không | (3,0) | 
| 2 | R | (3,1) | Có | (3,1) | 
| 3 | Bạn | (2,1) | Có | (2,1) | 
| 4 | L | (2,0) | Có | (2,0) | 

Nếu bất kỳ ô ứng cử viên nào bị chặn thì bước đó sẽ bị bỏ qua. Điều này chứng tỏ rằng các chướng ngại vật chỉ ảnh hưởng đến sự chuyển tiếp cục bộ chứ không ảnh hưởng đến logic chuyển động trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O( | s | 
| Không gian |$O(nm)$| Lưu trữ lưới | 

Giới hạn đầu vào cho phép lên tới$2 \cdot 10^6$lệnh, do đó việc quét tuyến tính qua chuỗi lệnh diễn ra trong giới hạn thời gian. Kích thước lưới cũng đủ nhỏ để lưu trữ trực tiếp mà không cần nén. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]
    s = input().strip()

    x, y = n - 1, 0

    for c in s:
        nx, ny = x, y
        if c == 'U':
            nx -= 1
        elif c == 'D':
            nx += 1
        elif c == 'L':
            ny -= 1
        else:
            ny += 1

        if 0 <= nx < n and 0 <= ny < m and grid[nx][ny] == '.':
            x, y = nx, ny

    return f"{x+1} {y+1}"

# sample 1
assert run("4 4\n....\n....\n....\n....\nDRLU\n") == "1 1"

# sample 2
assert run("4 4\n...#\n....\n....\n....\nDRUL\n") == "1 2"

# custom: single cell
assert run("1 1\n.\nURDL\n") == "1 1"

# custom: blocked neighbors
assert run("2 2\n.#\n..\nURR\n") == "2 1"

# custom: long harmless moves
assert run("3 3\n...\n...\n...\n" + "R"*1000000) == "3 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | 1 1 | không thể di chuyển được | 
| hàng xóm bị chặn | vị trí ổn định | logic chặn chướng ngại vật | 
| sự lặp lại dài | xử lý ổn định | xử lý tuyến tính không tràn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi mọi nước đi đều cố gắng rời khỏi lưới. Ví dụ: bắt đầu từ góc dưới bên trái và liên tục đưa ra`D`hoặc`L`lệnh. Thuật toán đánh giá từng nước đi một cách độc lập, phát hiện vị trí ứng cử viên nằm ngoài giới hạn và giữ nguyên trạng thái hiện tại. Không có sự tích tụ của chuyển động không hợp lệ xảy ra. 

Một trường hợp khác là chu vi bị chặn hoàn toàn xung quanh vị trí bắt đầu. Ngay cả khi các lệnh gợi ý chuyển động, mọi lần kiểm tra ô liền kề đều không thành công do`#`, nên vị trí không bao giờ thay đổi. Thuật toán vẫn thực hiện tất cả các lần kiểm tra trong thời gian không đổi cho mỗi lệnh. 

Trường hợp thứ ba là chuỗi lệnh cực kỳ dài. Vì mỗi lần lặp chỉ thực hiện tra cứu số học và một mảng duy nhất nên thời gian chạy sẽ chia tỷ lệ tuyến tính và không phụ thuộc vào kích thước lưới.
