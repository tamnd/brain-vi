---
problem: 897A
contest_id: 897
problem_index: A
name: "Scarborough Fair"
contest_name: "Codeforces Round 449 (Div. 2)"
rating: 800
tags: ["implementation"]
answer: passed_samples
verified: true
solve_time_s: 75
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
---

#CF 897A - Hội chợ Scarborough 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có (1/1 mẫu) 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi bắt đầu gồm các chữ cái viết thường và sau đó là một chuỗi các thao tác sửa đổi nó nhiều lần. Mỗi thao tác xác định một đoạn của chuỗi, từ vị trí`l`ĐẾN`r`, và một cặp ký tự`c1`Và`c2`. Bên trong phân khúc đó, mỗi lần xuất hiện của`c1`được thay thế bởi`c2`, trong khi tất cả các ký tự khác không thay đổi. 

Điểm mấu chốt là mỗi hoạt động đều có chọn lọc. Nó không ghi đè lên toàn bộ phân đoạn, nó chỉ chuyển đổi ký tự này thành ký tự khác ở bất kỳ nơi nào nó xuất hiện trong phạm vi nhất định. Vì các thao tác sau này sẽ tác động lên chuỗi đã được sửa đổi nên các phép biến đổi sẽ tích lũy theo thời gian. 

Các ràng buộc rất nhỏ: cả độ dài chuỗi và số phép toán nhiều nhất là 100. Điều này ngay lập tức cho chúng ta biết rằng ngay cả các giải pháp bậc hai hoặc bậc ba đều an toàn. Mọi thao tác lên tới khoảng 10^6 đều không đáng kể trong Python, vì vậy chúng tôi không cần các cấu trúc dữ liệu nâng cao hoặc thủ thuật tối ưu hóa. Một mô phỏng trực tiếp là đủ. 

Nguyên nhân chính thường xảy ra sai lầm là hiểu sai bản chất có điều kiện của việc thay thế. Một sai lầm ngây thơ là thay thế mọi ký tự trong`[l, r]`với`c2`, thay vì chỉ thay thế những giá trị bằng`c1`. 

Ví dụ, nếu chuỗi là`abca`và chúng tôi áp dụng`(1, 4, a, b)`, chỉ có`a`nhân vật nên thay đổi, sản xuất`bbc b`(tức là,`bbcb`). Việc thay thế toàn bộ phân đoạn sẽ đưa ra kết quả không chính xác`bbbb`. 

Một vấn đề tế nhị khác là lập chỉ mục. Sự cố sử dụng lập chỉ mục dựa trên 1, nhưng chuỗi Python dựa trên 0. Việc không điều chỉnh các chỉ số sẽ dẫn đến sai sót từng lỗi một, đặc biệt ở các ranh giới như`l = 1`hoặc`r = n`. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là mô phỏng từng thao tác trực tiếp trên một biểu diễn có thể thay đổi của chuỗi. Chúng tôi chuyển đổi chuỗi thành danh sách các ký tự để việc cập nhật có hiệu quả. 

Đối với mỗi hoạt động, chúng tôi quét phân đoạn`[l-1, r-1]`và kiểm tra từng ký tự. Nếu nó khớp`c1`, chúng tôi thay thế nó bằng`c2`. Nếu không, chúng tôi để nó không thay đổi. Điều này đúng vì định nghĩa vấn đề hoàn toàn mang tính cục bộ và không liên quan đến sự phụ thuộc giữa các vị trí khác nhau. 

Cách tiếp cận bạo lực này thực hiện nhiều nhất`m * n`kiểm tra ký tự. Với`n, m ≤ 100`, đây nhiều nhất là 10.000 thao tác, hoàn toàn an toàn. 

Không cần phải xử lý trước hoặc cấu trúc dữ liệu như cây phân đoạn vì các bản cập nhật không phải là phép gán hoặc tập hợp phạm vi. Chúng là sự thay thế ký tự có điều kiện đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Đã chấp nhận | 
| Tối ưu (cùng ý tưởng) | O(nm) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi và chuyển nó thành danh sách để có thể sửa đổi các ký tự tại chỗ. Điều này tránh việc tái cấu trúc chuỗi lặp đi lặp lại, điều này sẽ không hiệu quả trong Python. 
2. Đối với mỗi thao tác`(l, r, c1, c2)`, điều chỉnh các chỉ số về dạng dựa trên 0 bằng cách trừ 1 từ`l`Và`r`. Điều này đảm bảo căn chỉnh chính xác với lập chỉ mục Python. 
3. Lặp lại mọi vị trí từ`l`ĐẾN`r`. Tại mỗi chỉ mục, kiểm tra xem ký tự hiện tại có bằng không`c1`. 
4. Nếu trùng khớp, hãy thay thế bằng`c2`. Nếu không khớp thì giữ nguyên. Việc kiểm tra có điều kiện này rất cần thiết vì hoạt động này mang tính chọn lọc chứ không phải toàn bộ. 
5. Sau khi xử lý tất cả các thao tác, nối danh sách lại thành một chuỗi và xuất ra. 

### Tại sao nó hoạt động 

Mỗi thao tác được áp dụng chính xác theo thứ tự nhất định và mỗi ký tự được cập nhật độc lập chỉ dựa trên giá trị hiện tại của nó và liệu nó có nằm trong phạm vi hoạt động hay không. Thuật toán duy trì tính bất biến là sau khi xử lý thao tác thứ k, chuỗi phản ánh chính xác tác động của k phép biến đổi đầu tiên được áp dụng tuần tự. Vì mọi thao tác đều được thực hiện chính xác như đã xác định nên không có tương tác ngoài ý muốn nào xảy ra giữa các vị trí không liên quan. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    s = list(input().strip())

    for _ in range(m):
        l, r, c1, c2 = input().split()
        l = int(l) - 1
        r = int(r) - 1

        for i in range(l, r + 1):
            if s[i] == c1:
                s[i] = c2

    print("".join(s))

if __name__ == "__main__":
    main()
```Giải pháp sử dụng danh sách thay vì chuỗi vì chuỗi Python là bất biến, nghĩa là mọi sửa đổi sẽ tạo ra một chuỗi mới. Điều đó vẫn sẽ xảy ra ở đây, nhưng cách tiếp cận danh sách rõ ràng hơn và tránh việc phân bổ lặp lại. 

Việc xử lý ranh giới rất đơn giản:`r + 1`được sử dụng trong vòng lặp để bao gồm điểm cuối bên phải. Cả hai trừ 1`l`Và`r`đảm bảo chuyển đổi chính xác từ lập chỉ mục dựa trên 1. 

Mỗi thao tác được áp dụng ngay lập tức, duy trì ngữ nghĩa tuần tự mà bài toán yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
ioi
1 1 i n
```Chuỗi ban đầu:`ioi`| Bước | Phạm vi | Hành động | Chuỗi | 
| --- | --- | --- | --- | 
| 1 | [1,1] | tôi → n | noi | 

Sau khi chỉ kiểm tra ký tự đầu tiên, chỉ`i`tại vị trí 1 được thay đổi, tạo ra`noi`. 

Điều này xác nhận rằng thuật toán chỉ áp dụng chính xác các thay thế trong phạm vi và chỉ cho các ký tự trùng khớp. 

### Ví dụ 2 

đầu vào:```
5 3
wxxak
1 3 w x
2 4 x a
1 5 a g
```| Bước | Phạm vi | Hành động | Chuỗi | 
| --- | --- | --- | --- | 
| 0 | - | ban đầu | wxxak | 
| 1 | [1,3] | w → x | xxxak | 
| 2 | [2,4] | x → a | xaaak | 
| 3 | [1,5] | a → g | xgggk | 

Mỗi thao tác được xây dựng dựa trên kết quả trước đó và chỉ các ký tự được nhắm mục tiêu mới được thay đổi. Dấu vết này cho thấy mô hình cập nhật tuần tự là cần thiết vì các hoạt động sau này phụ thuộc vào các sửa đổi trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi thao tác quét tối đa n ký tự và có m thao tác | 
| Không gian | O(n) | Chuỗi được lưu trữ dưới dạng danh sách có thể thay đổi có độ dài n | 

Với`n, m ≤ 100`, số lần kiểm tra ký tự tối đa là 10.000, con số này không đáng kể trong Python. Việc sử dụng bộ nhớ cũng ở mức tối thiểu vì chỉ lưu trữ biểu diễn chuỗi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    s = list(input().strip())

    for _ in range(m):
        l, r, c1, c2 = input().split()
        l = int(l) - 1
        r = int(r) - 1
        for i in range(l, r + 1):
            if s[i] == c1:
                s[i] = c2

    return "".join(s)

# provided sample
assert run("3 1\nioi\n1 1 i n\n") == "noi"

# all same character replacement
assert run("4 2\naaaa\n1 4 a b\n1 4 b c\n") == "cccc"

# partial range no effect case
assert run("5 1\nabcde\n2 4 z x\n") == "abcde"

# boundary replacement
assert run("5 1\nabcda\n1 5 a z\n") == "zbcdz"

# single character string
assert run("1 3\na\n1 1 a b\n1 1 b c\n1 1 c d\n") == "d"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều giống nhau | cccc | thay thế xích | 
| không khớp | abcde | hoạt động không làm gì | 
| ranh giới | zbcdz | độ chính xác toàn dải | 
| chuỗi char đơn | d | cập nhật lặp đi lặp lại trên một vị trí | 

## Vỏ cạnh 

Trường hợp một cạnh là khi`l == r`, nghĩa là thao tác chỉ ảnh hưởng đến một vị trí duy nhất. Ví dụ,`s = "abc"`, hoạt động`(2,2,'b','x')`. Bộ thuật toán`l = 1`-dựa vào`1`hiệu chỉnh dựa trên, do đó nó chỉ kiểm tra chỉ mục 1 ở dạng dựa trên 0. Vòng lặp chạy một lần và chỉ cập nhật chính xác ký tự đó, tạo ra`"axc"`. 

Một trường hợp khác là khi`c1 == c2`. Ví dụ,`(1,3,'a','a')`TRÊN`"aba"`. điều kiện`if s[i] == c1`vẫn giữ nguyên, nhưng việc thay thế bằng ký tự tương tự sẽ giữ nguyên chuỗi. Thuật toán xử lý việc này một cách tự nhiên mà không cần sử dụng cách viết hoa đặc biệt. 

Trường hợp cuối cùng là khi không có ký tự nào trong phân đoạn trùng khớp`c1`. Vì`"abc"`với`(1,3,'z','a')`, điều kiện không bao giờ được kích hoạt nên chuỗi không thay đổi. Quá trình quét vẫn chạy trên phân đoạn nhưng không có sửa đổi nào xảy ra, duy trì tính chính xác.
