# TCP/IP
## βΆ π TCPλ?
> λ€νΈμν¬ ν΅μ μμ μ λ’°μ μΈ μ°κ²°λ°©μ

### π νΉμ§
- μ λ’°μ± λ³΄μ₯
- μ°κ²° μ§ν₯μ 
- νλ¦μ μ΄μ νΌμ‘μ μ΄λ₯Ό μ§μν΄ λ°μ΄ν° μμλ₯Ό λ³΄μ₯νλ€.
`β νλ¦ μ μ΄ : μ‘μ  μΈ‘κ³Ό μμ  μΈ‘μ λ°μ΄ν° μ²λ¦¬ μλ μ°¨μ΄λ₯Ό μ‘°μ `
`β νΌμ‘ μ μ΄ : λ€νΈμν¬ λ΄μ ν¨ν· μκ° λμΉμ§ μκ² λ°©μ§`


### νλ¦ μ μ΄ λ°©μ
- Stop and Wait : λ§€λ² μ μ‘ν ν¨ν·μ λν΄ νμΈ μλ΅μ λ°μ ν λ€μ ν¨ν·μ μ μ‘νλ λ°©λ²
- Sliding Window : μμ μΈ‘μμ μ€μ ν μλμ° ν¬κΈ°λ§νΌ μ‘μ μΈ‘μμ νμΈμλ΅ μμ΄ μΈκ·Έλ¨ΌνΈλ₯Ό μ μ‘ν  μ μκ² ν΄ λ°μ΄ν° νλ¦μ λμ μΌλ‘ μ‘°μ νλ μ μ΄κΈ°λ²


### νΌμ‘ μ μ΄ λ°©μ
1. AIMD
- ν¨ν·μ νλμ© λ³΄λ΄κ³  λ¬Έμ μμ΄ λμ°©νλ©΄ window ν¬κΈ°λ₯Ό 1μ© μ¦κ°μμΌ μ μ‘νλ λ°©λ²
- ν¨ν· μ μ‘μ μ€ν¨νκ±°λ μΌμ  μκ°μ λκΈ°λ©΄ ν¨ν· μ μ‘ μλλ₯Ό μ λ°μΌλ‘ μ€μ
- λ¨μ  : λ€νΈμν¬κ° νΌμ‘ν΄μ§λ μν©μ λ―Έλ¦¬ κ°μ§νμ§ λͺ»ν¨, μ²μμ μ μ‘ μλλ₯Ό μ¬λ¦¬λ λ° μκ°μ΄ μ€λ κ±Έλ¦Ό

2. Slow Start
- ν¨ν·μ νλμ© λ³΄λ΄λ€κ° λ¬Έμ μμ΄ λμ°©νλ©΄ ACK ν¨ν·λ§λ€ window ν¬κΈ°λ₯Ό 1μ© μ¦κ°μμΌ μ μ‘νλ λ°©λ²
- νΌμ‘ νμμ΄ λ°μνλ©΄ window ν¬κΈ°λ₯Ό 1λ‘ λ¨μ΄λ¨λ¦Ό
- ν λ² νΌμ‘ νμμ΄ λ°μνκ³  λλ©΄ λ€νΈμν¬μ μμ©λ μμ κ°λ₯

3. Fast Retransmit (λΉ λ₯Έ μ¬μ μ‘)
- ν¨ν·μ λ°λ μͺ½μμλ λ¨Όμ  λμ°©ν΄μΌ ν  ν¨ν·μ΄ λμ°©νμ§ μκ³  λ€μ ν¨ν·μ΄ λμ°©ν κ²½μ°μ μ λμ°©ν κ²½μ°μ ACK ν¨ν·μ μ μ‘νλ€. μ΄λ, μ€κ°μ ν¨ν·μ΄ μμ€λλ©΄ μ‘μ  μΈ‘μμλ μλ²μ΄ μ€λ³΅λ ACK ν¨ν·μ λ°κ² λλ€. μ΄ μκ° λ¬Έμ κ° λλ μλ²μ ν¨ν·μ μ¬μ μ‘νλ€. 

4. Fast Recovery (λΉ λ₯Έ νλ³΅)
- νΌμ‘ν μνκ° λλ©΄ windown ν¬κΈ°λ₯Ό 1μ΄ μλ λ°μΌλ‘ μ€μ΄κ³  μ ν μ¦κ°μν€λ λ°©λ²



## βΆ π 3-way handshakingμ΄λ?
> TCPκ° μ νν μ μ‘μ μν΄ λΌλ¦¬μ μΈ μ μμ μ±λ¦½νλ €κ³  μ§ννλ κ³Όμ 
1. ν΄λΌμ΄μΈνΈκ° μλ²μκ² SYN ν¨ν· μ μ‘
2. μλ²κ° SYNμ λ°κ³  ν΄λΌμ΄μΈνΈμκ² λ°μλ€λ μ νΈμΈ ACKμ SYN ν¨ν· μ μ‘
3. ν΄λΌμ΄μΈνΈλ μλ²μκ² μλ΅μΌλ‘ ACK ν¨ν· μ μ‘


## βΆ π 4-way handshakingμ΄λ?
> ν΅μ μ΄ λλ ν ν΄μ νλ κ³Όμ 
1. ν΄λΌμ΄μΈνΈλ μλ²μκ² μ°κ²°μ μ’λ£νλ€λ FIN νλκ·Έ μ μ‘
2. μλ²λ FINμ λ°κ³  νμΈνλ€λ ACK μ μ‘
- μ΄λ, λͺ¨λ  λ°μ΄ν°λ₯Ό λ³΄λ΄κΈ° μν΄ CLOSE_WAIT μνκ° λ¨
3. μλ²λ λ°μ΄ν°λ₯Ό λͺ¨λ μ μ‘νλ€λ©΄ μ°κ²°μ΄ μ’λ£λμλ€λ FIN νλκ·Έλ₯Ό ν΄λΌμ΄μΈνΈμκ² μ μ‘
- μλ²λ‘λΆν° λͺ» λ°μ λ°μ΄ν°κ° μμ μ μκΈ° λλ¬Έμ TIME_WAITμ ν΅ν΄ κΈ°λ€λ¦Ό
4. ν΄λΌμ΄μΈνΈλ FINμ λ°κ³  νμΈνλ€λ ACKλ₯Ό μλ²μ μ μ‘

- μλ²λ ACKλ₯Ό λ°μ ν μμΌμ λ«λλ€.
- ν΄λΌμ΄μΈνΈλ TIME_WAIT μκ°μ΄ λλλ©΄ μμΌμ λ«λλ€.


## βΆ π UDPλ?
- TCPμ λ€λ₯΄κ² λ©μμ§λ₯Ό ν¨ν·μΌλ‘ λλκ³  μ¬μ‘°λ¦½νλ μλΉμ€λ₯Ό μ κ³΅νμ§ μμ
- λ°μ΄ν°λ₯Ό μ£Όκ³  λ°μ μ»΄ν¨ν°λΌλ¦¬ μ§μ  μ°κ²°ν  λ UDP μ¬μ©
- TCPλ³΄λ€ μλκ° λΉ λ₯΄κΈ° λλ¬Έμ λ°μ΄ν° μ μ€μ΄ λ°μν΄λ μκ΄μλ μ€νΈλ¦¬λ°, νλ©΄ μ μ‘ λ±μ μ¬μ©


### Reference
https://gyoogle.dev/blog/interview/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC.html