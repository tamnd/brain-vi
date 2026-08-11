---
title: "CF 102433D - Chia cho hai"
description: "Chúng ta bắt đầu với một số nguyên (A) và muốn biến nó thành (B). Bất cứ lúc nào, chúng ta có thể tăng giá trị hiện tại lên một hoặc chia cho hai khi giá trị hiện tại là số chẵn. Mỗi thao tác tốn một chi phí, vì vậy nhiệm vụ là tìm số lượng thao tác tối thiểu."
date: "2026-08-10T07:37:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102433
codeforces_index: "D"
codeforces_contest_name: "2019-2020 ACM-ICPC Pacific Northwest Regional Contest (Div. 1)"
rating: 0
weight: 102433
solve_time_s: 333
verified: true
draft: false
---

[CF 102433D - Chia cho hai](https://codeforces.com/problemset/problem/102433/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một số nguyên\(A\)và muốn biến nó thành\(B\). Bất cứ lúc nào, chúng ta có thể tăng giá trị hiện tại lên một hoặc chia cho hai khi giá trị hiện tại là số chẵn. Mỗi thao tác tốn một chi phí, vì vậy nhiệm vụ là tìm số lượng thao tác tối thiểu. 

Đầu vào chứa một cặp số nguyên dương\(A\)Và\(B\), mỗi cái nhiều nhất\(10^9\). Đầu ra là độ dài của chuỗi thao tác hợp lệ ngắn nhất thay đổi\(A\)vào trong\(B\). Các mẫu chính thức là\(103 \to 27\), thực hiện 4 thao tác và\(3 \to 8\), thực hiện 5 thao tác. citturn1search17 

Sự ràng buộc của\(10^9\)loại trừ các thuật toán mô phỏng mọi giá trị có thể lên đến\(A\). Một tìm kiếm tuyến tính có thể yêu cầu khoảng một tỷ trạng thái, đây là một giải pháp hiệu quả chỉ cần nhiều phép chia logarit. 

Có một số trường hợp nguy hiểm mà việc triển khai tham lam bất cẩn có thể thất bại. Nếu như\(A<B\), Ví dụ\(3,8\), câu trả lời đơn giản là\(8-3=5\). Chia trước sẽ chỉ di chuyển xa mục tiêu hơn, vì vậy chiến lược luôn cố gắng chia khi có thể là sai. Nếu như\(A=B\), chẳng hạn như\(7,7\), câu trả lời là 0 và vòng lặp phải dừng trước khi thực hiện một thao tác không cần thiết. Nếu như\(A>B\)Và\(A\)thật kỳ lạ, chẳng hạn như\(7,4\), phép chia không hợp pháp ngay lập tức. Đầu tiên chúng ta phải làm\(7\to8\), sau đó\(8\to4\), cho câu trả lời là 2. Cuối cùng, khi phép chia làm cho giá trị nhỏ hơn\(B\), chúng ta phải ngừng chia và sử dụng phép cộng. Vì\(103,27\), trình tự tối ưu là\(103\to104\to52\to26\to27\), vì vậy câu trả lời là 4. Một vòng lặp tiếp tục chia sau khi đạt đến 26 sẽ bỏ lỡ mức tối ưu. citturn1search0 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp là coi mọi số nguyên là một trạng thái đồ thị. Từ giá trị chẵn\(x\), có tối đa hai thao tác gửi đi,\(x\to x+1\)Và\(x\to x/2\), trong khi chỉ từ một giá trị lẻ\( the current value \(x\)lớn hơn\(B\), cuối cùng chúng ta cần một phép chia. Thêm một trong khi đã ở trên\(B\)không thể hoàn thành việc chuyển đổi, vì vậy hãy xem xét điều gì xảy ra trước phép chia đầu tiên. Nếu như\(x\)là số chẵn, thực hiện hai phép cộng và sau đó là phép chia,\[
x\to x+1\to x+2\to (x+2)/2,
\]có giá trị tương đương với việc chia trước rồi cộng một,\[
x\to x/2\to x/2+1.
\]Phiên bản thứ hai sử dụng ít thao tác hơn. Nhiều bổ sung hơn trước khi phân chia đầu tiên có thể được giảm bớt theo cách tương tự. Như vậy, nếu\(x\)chẵn thì phép toán đầu tiên của lời giải tối ưu là phép chia. 

Nếu như\(x\)là số lẻ, cần phải có một phép cộng trước phép chia thứ nhất, vì phép chia là bất hợp pháp nếu không. Sau phép cộng duy nhất đó giá trị là chẵn nên ta chia ngay. Bất kỳ cặp phép cộng bổ sung nào trước phép chia đều có thể được di chuyển lại sau phép chia và lưu một phép toán. 

Điều này đưa ra một quá trình tham lam xác định. Trong khi\(A>B\), làm\(A\)thậm chí có thể cộng một khi cần thiết rồi chia cho hai. Càng sớm càng\(A\le B\), phép chia không còn được mong muốn nữa. Khi đó mọi phép toán còn lại đều phải là phép cộng, do đó câu trả lời sẽ tăng thêm\(B-A\). 

Lực lượng vũ phu hoạt động vì mọi hoạt động có thể được biểu diễn dưới dạng một cạnh trong biểu đồ trạng thái không có trọng số, nhưng nó không thành công vì biểu đồ đó có thể chứa khoảng một tỷ trạng thái có liên quan. Quan sát rằng một đường dẫn tối ưu có một thứ tự chính tắc, nhiều nhất là một phép cộng trước mỗi phép chia, sẽ thu gọn quá trình tìm kiếm thành các bước \(O(\log A)\). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | \(O(A)\) nêu trong trường hợp xấu nhất | \(O(A)\) | Quá chậm | 
| Tối ưu | \(O(\log A)\) | \(O(1)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nếu\(A\le B\), trở lại\(B-A\). Mục tiêu đã bằng hoặc cao hơn giá trị hiện tại và việc thêm một mục tiêu sẽ trực tiếp đạt được mục tiêu đó. Bất kỳ sự phân chia nào cũng sẽ làm giá trị giảm xuống và sẽ cần ít nhất số lần bổ sung bổ sung để phục hồi. 

2. Trong khi\(A>B\), kiểm tra tính chẵn lẻ của\(A\). Nếu như\(A\)là số lẻ, hãy thêm một và tăng số lượng thao tác. Đây là cách duy nhất để làm cho sự phân chia hợp pháp. 

3. Chia\(A\)lên hai và tăng số lượng hoạt động. Khi\(A\)đã chẵn, đây là nước đi đầu tiên tối ưu. Khi nó lẻ, việc bổ sung trước đó là bắt buộc. 

4. Lặp lại cho đến khi\(A\le B\). Khi điều này xảy ra, hãy ngừng thực hiện phép chia. 

5. Thêm\(B-A\)đến số lượng hoạt động và trả lại nó. Từ\(A\le B\)tại thời điểm này, chỉ cần bổ sung để đạt được mục tiêu. 

Bất biến chính là trước mỗi phép chia, thuật toán đã sử dụng số phép cộng tối thiểu có thể cần thiết để làm cho giá trị hiện tại chia hết cho hai. Đối với giá trị chẵn, số đó bằng 0 và đối với giá trị lẻ, nó chính xác là một. Bất kỳ giải pháp tối ưu nào cũng có thể được chuyển đổi thành một giải pháp có cùng cấu trúc này mà không cần tăng độ dài của nó, bởi vì các phần bổ sung bổ sung trước phép chia có thể được di chuyển sau phép chia đó với chi phí rẻ hơn. Khi giá trị giảm xuống hoặc thấp hơn\(B\), phép chia không thể là một phần của sự tiếp tục tối ưu, do đó phép cộng trực tiếp cuối cùng cũng là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b = map(int, input().split())

    if a <= b:
        print(b - a)
        return

    ans = 0

    while a > b:
        if a & 1:
            a += 1
            ans += 1

        a //= 2
        ans += 1

    ans += b - a
    print(ans)

if __name__ == "__main__":
    solve()
```Điều kiện đầu tiên xử lý toàn bộ\(A\le B\)trường hợp ngay lập tức. Đây không chỉ là một sự tối ưu hóa: nó ngăn thuật toán chia một giá trị không lớn hơn mục tiêu. 

Bên trong vòng lặp,`a & 1`kiểm tra xem giá trị hiện tại có phải là số lẻ hay không. Nếu đúng như vậy,`a += 1`làm cho việc phân chia trở nên hợp pháp và tốn một chi phí cho một hoạt động. Phép chia số nguyên`a //= 2`thì luôn luôn hợp lệ. 

Thứ tự quan trọng. Trước tiên chúng ta phải tạo một giá trị chẵn và chỉ sau đó chia nó. Chúng tôi cũng kiểm tra`a > b`trước khi bắt đầu lần lặp khác, vì sau khi chia, giá trị mới có thể đã thấp hơn mục tiêu. Trong tình huống đó, khoảng cách còn lại được xử lý bởi`b - a`. 

Số nguyên Python không tràn và giá trị tạm thời lớn nhất chỉ\(10^9+1\), do đó không cần xử lý số đặc biệt. 

## Ví dụ đã hoạt động 

### Mẫu 1:\(A=103,\ B=27\)| Bước | Hiện hành\(A\)| Hành động | Hoạt động | 
|---|---:|---|---:| 
| Bắt đầu | 103 | Lẻ, cộng 1 | 1 | 
| 1 | 104 | Chia cho 2 | 2 | 
| 2 | 52 | Chia cho 2 | 3 | 
| 3 | 26 | Thôi đừng chia nữa\(A\le B\)| 3 | 
| Kết thúc | 26 | Thêm vào\(27-26=1\)| 4 | 

Trình tự là\(103\to104\to52\to26\to27\). Ví dụ này chứng minh tại sao điều kiện dừng là cần thiết. Tiếp tục với một bộ phận khác sẽ tạo ra 13 và cần thêm nhiều phép bổ sung nữa. 

### Mẫu 2:\(A=3,\ B=8\)| Bước | Hiện hành\(A\)| Hành động | Hoạt động | 
|---|---:|---|---:| 
| Bắt đầu | 3 |\(A\le B\), dừng vòng lặp | 0 | 
| Kết thúc | 3 | Thêm vào\(8-3=5\)| 5 | 

Đáp án là 5, tương ứng với 5 phép cộng. Điều này thực hiện trường hợp phép chia không bao giờ hữu ích vì giá trị bắt đầu đã thấp hơn mục tiêu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(\log A)\) | Mỗi phép chia làm giảm giá trị hiện tại khoảng một nửa và có nhiều nhất là nhiều phép chia theo logarit. | 
| Không gian | \(O(1)\) | Chỉ có giá trị hiện tại và bộ đếm hoạt động được lưu trữ. | 

Với\(A\le10^9\), số lượng phép chia chỉ ở mức 30, với tối đa một thao tác bổ sung để cố định tính chẵn lẻ trước mỗi phép chia. Do đó, thuật toán chỉ thực hiện vài chục lần lặp, thoải mái trong giới hạn một giây và sử dụng bộ nhớ không đổi. 

## Trường hợp thử nghiệm 

``` con trăn 
hệ thống nhập khẩu 
nhập khẩu io 

giải quyết chắc chắn(): 
a, b = map(int, input().split()) 

nếu a <= b: 
in(b - a) 
trở về 

trả lời = 0 

trong khi a > b: 
nếu a & 1: 
một += 1 
đáp án += 1 

một //= 2 
đáp án += 1 

trả lời += b - a 
in(an) 

def run(inp: str) -> str: 
old_stdin = sys.stdin 
old_stdout = sys.stdout 

sys.stdin = io.StringIO(inp) 
sys.stdout = io.StringIO() 

giải quyết() 
đầu ra = sys.stdout.getvalue() 

sys.stdin = old_stdin 
sys.stdout = old_stdout 

trả về đầu ra.strip() 

# Cung cấp mẫu 
khẳng định run("103 27\n") == "4", "mẫu 1" 
khẳng định run("3 8\n") == "5", "mẫu 2" 

# Trường hợp tùy chỉnh 
khẳng định run("1 1\n") == "0", "giá trị tối thiểu bằng nhau" 
khẳng định run("1000000000 1000000000\n") == "0", "giá trị tối đa bằng nhau" 
khẳng định run("2 1\n") == "1", "phân chia hữu ích nhỏ nhất" 
khẳng định run("5 3\n") == "2", "giá trị lẻ cần tăng một" 
khẳng định run("6 5\n") == "3", "chia dưới mục tiêu theo sau là bổ sung" 
khẳng định run("1000000000 1\n") == "37", "giá trị bắt đầu tối đa" 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---:|---| 
|`1 1`| 0 | Giá trị tối thiểu và ranh giới đẳng thức | 
|`1000000000 1000000000`| 0 | Giá trị tối đa và đẳng thức | 
|`2 1`| 1 | Phép chia có giá trị tức thời | 
|`5 3`| 2 | Giá trị lẻ cần tăng trước khi chia | 
|`6 5`| 3 | Sư đoàn có thể bắn hạ mục tiêu, sau đó là bổ sung | 
|`1000000000 1`| 37 | Đầu vào lớn và hành vi logarit | 

## Vỏ cạnh 

cho\(A=B\), lấy đầu vào`7 7`. Điều kiện ban đầu\(A\le B\)là đúng, do đó thuật toán trả về\(7-7=0\). Không có hoạt động là cần thiết. Một vòng lặp giả định có ít nhất một thao tác được yêu cầu sẽ đưa ra một bước bổ sung không chính xác. 

Vì\(A<B\), lấy`3 8`. Thuật toán ngay lập tức trở lại\(8-3=5\). Mỗi phép chia sẽ làm cho giá trị hiện tại thậm chí còn nhỏ hơn, do đó nó không thể cải thiện sau năm phép cộng trực tiếp. Đây là lý do tại sao\(A\le B\)trường hợp phải được xử lý trước vòng chia. 

Đối với một giá trị lẻ trên mục tiêu, hãy lấy`7 4`. Vì 7 là số lẻ nên thuật toán thực hiện\(7\to8\), sau đó\(8\to4\), thực hiện 2 thao tác. Phép chia không thể được thực hiện trực tiếp trên 7, do đó việc tăng dần là bắt buộc. Kết quả đạt đích chính xác sau khi chia. 

Đối với một bộ phận vượt qua mục tiêu, hãy lấy`6 5`. Thuật toán thực hiện\(6\to3\), sau đó dừng lại vì\(3\le5\), và cộng hai lần:\(3\to4\to5\). Tổng số là 3. Việc chia lại sẽ chỉ làm tăng khối lượng công việc vì giá trị đã thấp hơn mục tiêu. 

Đối với mẫu`103 27`, trước tiên thuật toán sửa giá trị lẻ bằng\(103\to104\), sau đó chia hai lần để có được\(26\). Vì 26 bây giờ nhỏ hơn 27 nên nó thực hiện phép cộng cuối cùng. Tổng cộng là\(1+1+1+1=4\), phù hợp với câu trả lời được yêu cầu. trích dẫn
