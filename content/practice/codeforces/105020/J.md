---
title: "CF 105020J - Trốn Tìm"
description: "Mỗi trường hợp thử nghiệm mô tả một công viên có nhiều cây, trong đó mỗi cây có chiều cao cố định. Amr chọn một cái cây để trốn đằng sau. Việc anh ta có bị bắt hay không chỉ phụ thuộc vào sự so sánh giữa chiều cao của anh ta và chiều cao của cái cây đã chọn đó."
date: "2026-06-28T01:59:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105020
codeforces_index: "J"
codeforces_contest_name: "TCPC Tunisian Collegiate Programming Contest 2022"
rating: 0
weight: 105020
solve_time_s: 72
verified: false
draft: false
---

[CF 105020J - Trốn tìm](https://codeforces.com/problemset/problem/105020/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một công viên có nhiều cây, trong đó mỗi cây có chiều cao cố định. Amr chọn một cái cây để trốn đằng sau. Việc anh ta có bị bắt hay không chỉ phụ thuộc vào sự so sánh giữa chiều cao của anh ta và chiều cao của cái cây đã chọn đó. 

Luật chơi rất đơn giản: Gamal chỉ bắt Amr khi cái cây mà Amr đang ẩn nấp phía sau thấp hơn Amr. Nếu cây đủ cao hoặc có chiều cao bằng nhau thì Amr vẫn an toàn. 

Vì vậy, đối với mỗi cây, chúng ta được yêu cầu quyết định độc lập xem chiều cao của nó có nhỏ hơn một giá trị nhất định hay không$x$, đại diện cho chiều cao của Amr. Đầu ra là một chuỗi các quyết định nhị phân, mỗi quyết định một cây. 

Các ràng buộc cho phép lên đến$10^3$trường hợp thử nghiệm và tổng số$10^5$tổng thể cây cối. Điều này đã cho chúng ta biết giải pháp phải xử lý từng độ cao trong thời gian không đổi. Bất kỳ cách tiếp cận nào truy cập lại các phần tử nhiều lần cho mỗi trường hợp thử nghiệm vẫn sẽ chỉ vượt qua nếu nó vẫn tuyến tính về tổng thể. Bất cứ điều gì bậc hai, chẳng hạn như so sánh mọi cây với mọi cây khác, đều vượt quá giới hạn vì nó sẽ đạt tới$10^{10}$hoạt động trong trường hợp xấu nhất. 

Các trường hợp đặc biệt chính đến từ sự so sánh ranh giới xung quanh sự bình đẳng và các thái cực: 

Nếu tất cả các cây đều có chiều cao bằng$x$, không có con nào được tính là bị bắt. Ví dụ,$x = 5$, độ cao$[5, 5, 5]$, đầu ra phải là$[0, 0, 0]$. Việc thực hiện bất cẩn bằng cách sử dụng$\leq$thay vì so sánh nghiêm ngặt sẽ đánh dấu không chính xác tất cả là đã bắt được. 

Nếu tất cả các cây đều ngắn hơn$x$, Ví dụ$x = 10$, độ cao$[1, 2, 9]$, sau đó mọi vị trí sẽ trở lại$1$. Bất kỳ sai lầm nào trong việc đảo ngược điều kiện sẽ đảo ngược hoàn toàn kết quả. 

Nếu cây bao gồm các giá trị lớn hơn$x$, đó phải là các vị trí an toàn mặc dù chúng không bằng nhau, điều này có thể cản trở việc triển khai chỉ kiểm tra sự bằng nhau hoặc sử dụng logic boolean không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xử lý từng cây một cách độc lập. Đối với mỗi cây, chúng tôi so sánh chiều cao của nó với chiều cao của Amr và quyết định xem nó có thỏa mãn điều kiện “nhỏ hơn” hay không. Điều này đã đưa ra một giải pháp chính xác vì quyết định của một cây không phụ thuộc vào bất kỳ cây nào khác. 

Một sự hiểu lầm ngây thơ có thể là tính toán lại hoặc quét cấu trúc bổ sung trên mỗi cây, nhưng không có gì trong vấn đề yêu cầu sự tương tác giữa các vị trí. Ngay cả một vòng lặp kép mà đối với mỗi cây, so sánh với tất cả các cây khác cũng sẽ không cần thiết và lãng phí. Điều đó sẽ dẫn đến$O(n^2)$hành vi trên mỗi trường hợp thử nghiệm, trong trường hợp xấu nhất có nghĩa là xung quanh$10^{10}$so sánh và hoàn toàn không khả thi. 

Quan sát quan trọng là tình trạng này hoàn toàn mang tính cục bộ. Mỗi đầu ra chỉ phụ thuộc vào một so sánh duy nhất$h_i < x$. Điều này giúp loại bỏ sự cần thiết của bất kỳ cấu trúc dữ liệu hoặc tiền xử lý nào. Chúng ta có thể xử lý mảng trong một lần và đưa ra câu trả lời ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Mỗi trường hợp thử nghiệm là độc lập, do đó không cần phải chuyển thông tin qua chúng. 
2. Với mỗi test case, hãy đọc$n$Và$x$, xác định số lượng cây tồn tại và chiều cao của Amr. 
3. Đọc danh sách chiều cao của cây. Mỗi giá trị sẽ được kiểm tra chính xác một lần. 
4. Đối với mỗi chiều cao$h_i$, so sánh trực tiếp với$x$. Nếu như$h_i < x$, đầu ra 1, nếu không thì đầu ra 0. Điều này có tác dụng vì điều kiện bài toán xác định bất đẳng thức nghiêm ngặt là cách duy nhất để Amr bị bắt. 
5. In tất cả kết quả của ca kiểm thử trên một dòng duy nhất để khớp với định dạng được yêu cầu. 

### Tại sao nó hoạt động 

Mỗi quyết định là độc lập và chỉ dựa vào việc một số có nằm dưới ngưỡng cố định hay không$x$. Không có sự tương tác ẩn giữa các cây nên kết quả đầu ra chính xác là kết quả của việc áp dụng một vị từ đơn giản$h_i < x$tới mọi phần tử. Vì vị từ được đánh giá một lần cho mỗi phần tử nên không thể xảy ra sự ghép nối không chính xác giữa các vị trí và kết quả phải khớp với định nghĩa bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out_lines = []

    for _ in range(t):
        n, x = map(int, input().split())
        h = list(map(int, input().split()))

        res = []
        for hi in h:
            if hi < x:
                res.append("1")
            else:
                res.append("0")

        out_lines.append(" ".join(res))

    print("\n".join(out_lines))

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh chính xác thuật toán. Mỗi trường hợp thử nghiệm được xử lý riêng biệt và với mỗi chiều cao của cây, chúng tôi thực hiện một so sánh duy nhất. Kết quả được tích lũy dưới dạng chuỗi để tránh chi phí in lặp đi lặp lại, giúp giải pháp này hoạt động hiệu quả trong Python trong điều kiện hạn chế chặt chẽ. 

Một cạm bẫy phổ biến ở đây là vô tình sử dụng`<=`thay vì`<`, điều này sẽ đánh dấu không chính xác những cây có chiều cao bằng nhau là đã bị bắt. Một vấn đề nhỏ khác là in từng phần tử thay vì nối các kết quả, điều này có thể làm chậm đáng kể Python khi$n$là lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, x = 4
heights = [1, 4, 2, 7, 3]
```| tôi | h_i | h_i < x | đầu ra | 
| --- | --- | --- | --- | 
| 1 | 1 | đúng | 1 | 
| 2 | 4 | sai | 0 | 
| 3 | 2 | đúng | 1 | 
| 4 | 7 | sai | 0 | 
| 5 | 3 | đúng | 1 | 

Đầu ra:```
1 0 1 0 1
```Dấu vết này cho thấy sự bình đẳng với$x$không được tính là bị bắt, chỉ có vấn đề bất bình đẳng nghiêm ngặt. 

### Ví dụ 2 

đầu vào:```
n = 4, x = 6
heights = [6, 6, 1, 10]
```| tôi | h_i | h_i < x | đầu ra | 
| --- | --- | --- | --- | 
| 1 | 6 | sai | 0 | 
| 2 | 6 | sai | 0 | 
| 3 | 1 | đúng | 1 | 
| 4 | 10 | sai | 0 | 

Đầu ra:```
0 0 1 0
```Ví dụ này nhấn mạnh rằng các giá trị lớn hơn$x$cũng an toàn, củng cố rằng điều kiện nghiêm ngặt là ở dưới ngưỡng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi chiều cao của cây được kiểm tra chính xác một lần đối với$x$| 
| Không gian |$O(1)$phụ trợ | Chỉ lưu trữ đầu ra được sử dụng ngoài mảng đầu vào | 

Tổng số lần kiểm tra cây trên tất cả các trường hợp thử nghiệm nhiều nhất là$10^5$, phù hợp thoải mái trong giới hạn quét tuyến tính. Việc sử dụng bộ nhớ vẫn ở mức tối thiểu vì không cần cấu trúc bổ sung. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, x = map(int, input().split())
        h = list(map(int, input().split()))
        res = ["1" if hi < x else "0" for hi in h]
        out.append(" ".join(res))
    print("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.strip()

# provided sample (formatted consistently)
assert run("""1
5 4
1 4 2 7 3
""") == "1 0 1 0 1"

# all equal case
assert run("""1
3 5
5 5 5
""") == "0 0 0"

# all smaller
assert run("""1
4 10
1 2 3 4
""") == "1 1 1 1"

# all larger
assert run("""1
4 2
5 6 7 8
""") == "0 0 0 0"

# boundary mix
assert run("""1
5 3
3 2 3 1 4
""") == "0 1 0 1 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bằng x | tất cả 0 | tính đúng đắn của bất đẳng thức | 
| tất cả đều nhỏ hơn | tất cả 1 | trường hợp chấp nhận đầy đủ | 
| tất cả lớn hơn | tất cả 0 | trường hợp từ chối | 
| trộn ranh giới | hỗn hợp | logic từng phần tử đúng | 

## Vỏ cạnh 

Khi mọi cây đều có chiều cao chính xác bằng$x$, thuật toán đánh giá mỗi so sánh là sai, tạo ra một chuỗi các số 0. Đối với một đầu vào như$x = 5$, độ cao$[5, 5, 5]$, mỗi lần kiểm tra$h_i < x$không thành công nên không có vị trí nào được đánh dấu là bị bắt. 

Khi tất cả các cây đều ở phía dưới$x$, mọi so sánh đều thành công. Vì$x = 10$, độ cao$[1, 2, 9]$, mỗi so sánh trả về true và đầu ra trở thành tất cả. 

Khi giá trị vượt quá$x$, việc so sánh sẽ lọc chúng ra một cách tự nhiên mà không cần xử lý đặc biệt. Vì$x = 3$, độ cao$[4, 5]$, cả hai đều lớn hơn và do đó an toàn, tạo ra các số 0 một cách nhất quán.
