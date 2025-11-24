<?xml version="1.0" encoding="UTF-8"?>
<definitions xmlns="http://www.omg.org/spec/BPMN/20100524/MODEL"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:bpmndi="http://www.omg.org/spec/BPMN/20100524/DI"
             xmlns:omgdc="http://www.omg.org/spec/DD/20100524/DC"
             xmlns:omgdi="http://www.omg.org/spec/DD/20100524/DI"
             id="Definitions_Lean"
             targetNamespace="http://bpmn.io/schema/bpmn">

  <process id="Processo_Lean_Melhorias" name="Fluxo Lean com Melhorias" isExecutable="false">

    <!-- Start -->
    <startEvent id="StartEvent" name="Início do Processo"/>

    <!-- Análise inicial -->
    <task id="T1" name="Analisar situação atual do setup"/>
    <sequenceFlow id="F1" sourceRef="StartEvent" targetRef="T1"/>

    <!-- Identificação dos desperdícios -->
    <task id="T2" name="Identificar desperdícios Lean"/>
    <sequenceFlow id="F2" sourceRef="T1" targetRef="T2"/>

    <!-- Gateway para cada tipo de desperdício -->
    <exclusiveGateway id="GW1" name="Tipo de desperdício?"/>
    <sequenceFlow id="F3" sourceRef="T2" targetRef="GW1"/>

    <!-- MOVIMENTO EXCESSIVO -->
    <task id="T3" name="Aplicar Kanban e reorganizar layout (POU)"/>
    <sequenceFlow id="F4" sourceRef="GW1" targetRef="T3">
      <conditionExpression xsi:type="tFormalExpression">movimento</conditionExpression>
    </sequenceFlow>

    <!-- ESPERA -->
    <task id="T4" name="Implementar SMED e pré-setup"/>
    <sequenceFlow id="F5" sourceRef="GW1" targetRef="T4">
      <conditionExpression xsi:type="tFormalExpression">espera</conditionExpression>
    </sequenceFlow>

    <!-- FALTA DE PADRONIZAÇÃO -->
    <task id="T5" name="Criar SOP e treinar com TWI"/>
    <sequenceFlow id="F6" sourceRef="GW1" targetRef="T5">
      <conditionExpression xsi:type="tFormalExpression">padronizacao</conditionExpression>
    </sequenceFlow>

    <!-- DEFEITOS -->
    <task id="T6" name="Aplicar poka-yoke e calibrar parâmetros"/>
    <sequenceFlow id="F7" sourceRef="GW1" targetRef="T6">
      <conditionExpression xsi:type="tFormalExpression">defeitos</conditionExpression>
    </sequenceFlow>

    <!-- FALTA DE 5S -->
    <task id="T7" name="Executar 5S completo + shadow board"/>
    <sequenceFlow id="F8" sourceRef="GW1" targetRef="T7">
      <conditionExpression xsi:type="tFormalExpression">5s</conditionExpression>
    </sequenceFlow>

    <!-- Retorno ao fluxo -->
    <exclusiveGateway id="GW2" name="Há mais desperdícios?"/>
    <sequenceFlow id="F9" sourceRef="T3" targetRef="GW2"/>
    <sequenceFlow id="F10" sourceRef="T4" targetRef="GW2"/>
    <sequenceFlow id="F11" sourceRef="T5" targetRef="GW2"/>
    <sequenceFlow id="F12" sourceRef="T6" targetRef="GW2"/>
    <sequenceFlow id="F13" sourceRef="T7" targetRef="GW2"/>

    <!-- Loop -->
    <sequenceFlow id="F14" sourceRef="GW2" targetRef="T2">
      <conditionExpression xsi:type="tFormalExpression">sim</conditionExpression>
    </sequenceFlow>

    <!-- Encerramento -->
    <endEvent id="EndEvent" name="Processo melhorado"/>
    <sequenceFlow id="F15" sourceRef="GW2" targetRef="EndEvent">
      <conditionExpression xsi:type="tFormalExpression">nao</conditionExpression>
    </sequenceFlow>

  </process>

</definitions>
